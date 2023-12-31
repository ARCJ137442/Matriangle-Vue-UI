<template>
	<h2>控制器</h2>
	<div>
		<input
			type="text"
			v-model="controlAddress"
			placeholder="输入链接"
			@keydown="
				(e: KeyboardEvent): false | void =>
					e.key === 'Enter' && onAddressChange()
			"
		/>
		<input type="text" v-model="controlKey" placeholder="输入控制密钥" />
		<p>{{ controlMessage }}</p>
		<button
			type="button"
			ref="controlStatusButton"
			@click="switchControlStatus()"
			@vue:mounted="updateControlStatus(_controlStatus)"
		>
			正在加载
		</button>
	</div>
	<!-- 控制台指令（暂时用不到`@input`） -->
	<!-- ! 【2023-11-10 23:38:16】目前的问题：在`consoleCmd.value +=`后，滚动条无法加入到光标处 ! -->
	<textarea
		class="console"
		contenteditable="true"
		v-model="consoleCmd"
		ref="consoleElement"
		placeholder="输入控制台指令"
		@keydown="
			(e: KeyboardEvent): false | void => {
				if (e.key === 'Enter') {
					// 阻止默认按键
					e.preventDefault()
					// 执行指令
					onConsoleCmd()
				}
			}
		"
	/>
</template>

<script setup lang="ts">
import { Ref, ref } from 'vue'

// 定义元素 //
const controlAddress: Ref<string> = ref('127.0.0.1:3002')
let _lastAddress: string = controlAddress.value
const controlKey: Ref<string> = ref('Alpha')
const controlMessage: Ref<string> = ref('')
const controlStatusButton: Ref<HTMLElement | null> = ref(null)
const consoleCmd: Ref<string> = ref('')
const consoleElement: Ref<HTMLTextAreaElement | null> = ref(null)

// * 自定义事件发送 * //

/** 注册事件发送器 */
const emit = defineEmits(['message', 'link-change'])
/** 发送消息 */
function emitMessage(message: string): void {
	emit('message', {
		address: controlAddress.value,
		message,
	})
}

/**
 * 地址变更事件
 * * 格式：(旧地址：string, 新地址：string)
 */
function onAddressChange(): void {
	emit(
		'link-change',
		// 旧地址
		_lastAddress,
		// 新地址
		controlAddress.value
	)
	// 更新旧地址
	_lastAddress = controlAddress.value
}

/**
 * 控制台指令事件
 */
function onConsoleCmd(): void {
	if (consoleCmd.value.length !== 0) {
		// 以用户输入的最后一行作为消息，加上「指令」前缀`/`
		const lines = consoleCmd.value.split('\n')
		// console.log('控制台指令', '/' + lines[lines.length - 1])
		// 发送消息
		emitMessage('/' + lines[lines.length - 1])
	}
	// 这里不换行，只有回显（有回应）后才换行
}

/**
 * 控制台指令输出
 */
function onConsoleCmdOutput(out: string): void {
	// 追加到输入框，附加换行
	consoleCmd.value += '\n' + out + '\n'
	// ! Vue里textarea无法通过dom属性获取到内容，并且其scrollTop并不准确
	/* console.log(
		'控制台指令',
		consoleElement.value,
		consoleElement.value?.textContent, //
		consoleElement.value?.scrollHeight,
		consoleElement.value?.scrollTop
	) */
	consoleElement

	// 更新滚动
	updateScrollStatusRef(consoleElement)
}

/** 更新滚动状态到最底部 */
function updateScrollStatus(element: HTMLElement): void {
	// !【2023-11-10 23:50:17】目前来说，需要延迟一小会儿，因为滚动条更新不一定那么快
	setTimeout((): void => {
		// `scrollHeight`属性不一定精确
		element.scrollTop += 0x1000000
		element.scrollTop *= 2
	}, 100)
}
function updateScrollStatusRef(ref: Ref<HTMLElement | null>): void {
	if (ref.value !== null) updateScrollStatus(ref.value)
	else console.warn('滚动状态更新失败')
}

// 控制の消息 //

type PressedKeys = { [key: string]: KeyboardEvent }
const pressed: PressedKeys = {}
/**
 * 根据键位获取「动作信息」
 *
 * @param {KeyboardEvent} event 键盘事件
 * @param {boolean} isDown 是否按下
 * @returns WS信息
 */
function getControlMessage(
	event: KeyboardEvent,
	isDown: boolean
): string | undefined {
	if (!(event.code in pressed)) console.log(event)
	pressed[event.code] = event
	const action = getActionFromEvent(event, isDown)
	if (action === undefined) return undefined
	// 生成套接字消息
	return `${controlKey.value}|${action}`
}

/**
 * 根据键盘事件返回「玩家行动」
 *
 * @param {KeyboardEvent} keyboardEvent 键盘事件
 * @param {boolean} isDown 是否按下
 * @returns 对应的「玩家行动」值
 */
function getActionFromEvent(
	keyboardEvent: KeyboardEvent,
	isDown: boolean
): number | string | undefined {
	switch (keyboardEvent.code) {
		// X
		case 'KeyA':
		case 'ArrowLeft':
			return isDown ? -2 : undefined // 只有按下时才响应
		case 'KeyD':
		case 'ArrowRight':
			return isDown ? -1 : undefined // 只有按下时才响应
		// Y
		case 'KeyW':
		case 'ArrowUp':
			return isDown ? -4 : undefined // 只有按下时才响应
		case 'KeyS':
		case 'ArrowDown':
			return isDown ? -3 : undefined // 只有按下时才响应
		// Z
		case 'KeyQ':
			return isDown ? -6 : undefined // 只有按下时才响应
		case 'KeyE':
			return isDown ? -5 : undefined // 只有按下时才响应
		// W
		case 'KeyZ':
			return isDown ? -8 : undefined // 只有按下时才响应
		case 'KeyC':
			return isDown ? -7 : undefined // 只有按下时才响应
		// 用
		case 'Space':
			return isDown ? 'startUsing' : 'stopUsing'
		// 进
		case 'KeyX':
			return isDown ? 'moveForward' : undefined
	}
}

// 键盘事件 //

/**
 * 统一APP键盘事件（接收者）
 *
 * ! 注意：键盘事件的`key`、`code`是不一样的
 * * `key`对应的是「键的语义」而不关注「位置」，如`Shift`（左右都会触发）
 * * `code`在「语义的基础上」还关注「键的具体位置」，如`ShiftLeft`（只对应左Shift）
 * * `keyCode`为「待弃用内容」，不推荐使用
 *
 * 参考：MDN文档（[key](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent/key)、[code](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent/code)）
 *
 * @param {KeyboardEvent} event 键盘事件
 * @param {boolean} isDown 是否为「按下」事件
 */
function onKeyEvent(event: KeyboardEvent, isDown: boolean): void {
	// 指定指针
	keyEventHandler?.(event, isDown)
}
/** @type {(event:KeyboardEvent, isDown:boolean) => void} */
let keyEventHandler: (event: KeyboardEvent, isDown: boolean) => void

/**
 * 对接老式的「远控操作」
 * @param {KeyboardEvent} event 键盘事件
 * @param {boolean} isDown 是否为「按下」事件
 */
function handleMultiKeyController(event: KeyboardEvent, isDown: boolean): void {
	// 产生消息
	const message = getControlMessage(event, isDown)
	if (message === undefined) return
	controlMessage.value = `${isDown ? '↓' : '↑'} message = ${message}`
	// 阻止默认操作（不会造成画面滚动）
	event.preventDefault()
	// 发送请求 // !「断线重连」的操作要在后边做
	emitMessage(message)
}

/**
 * 呼叫「键控中心」
 * * 消息格式：`|+【按键代码】`（按下⇒前导空格）/`|【按键代码】`（释放⇒原样）
 *
 * ! 要「键位」code而非「键值」key
 *
 * @param {KeyboardEvent} event 键盘事件
 * @param {boolean} isDown 按键是否是「按下」（否则为「释放」）
 */
function handleKeyboardControlCenter(
	event: KeyboardEvent,
	isDown: boolean
): void {
	// 阻止默认操作
	event.preventDefault()
	// 生成特定消息
	_temp_callKeyboardControlCenter_message = `|${isDown ? '+' : ''}${
		event.code
	}`
	// 呈现
	controlMessage.value = `${
		isDown ? '↓' : '↑'
	} message = ${_temp_callKeyboardControlCenter_message}`
	// 发送
	emitMessage(_temp_callKeyboardControlCenter_message)
}
let _temp_callKeyboardControlCenter_message: string

// 控制状态 //
let _controlStatus = 0
const controlStatusInf = [
	{
		name: '操作模式：无（点击切换）',
		handler: (): undefined => void 0,
	},
	{
		name: '操作模式：指定玩家',
		handler: handleMultiKeyController,
	},
	{
		name: '操作模式：键控中心',
		handler: handleKeyboardControlCenter,
	},
]

function updateControlStatus(value: number): void {
	if (controlStatusButton.value)
		controlStatusButton.value.innerText = controlStatusInf[value].name
	else console.error('updateControlStatus: 未找到控制按钮！')
	keyEventHandler = controlStatusInf[value].handler
	console.log('updateControlStatus:', controlStatusInf[value].name)
}
function switchControlStatus(): void {
	_controlStatus++
	_controlStatus %= controlStatusInf.length
	updateControlStatus(_controlStatus)
}

defineExpose({
	onKeyEvent,
	/**
	 * 不使用「心跳机制」调用「消息回调」
	 */
	onMessage(message: string): void {
		// 前导`/`⇒指令输出更新
		if (message.startsWith('/')) onConsoleCmdOutput(message.slice(1))
		// 其它⇒暂时不做什么
	},
})
</script>

<style scoped>
#control {
	/* 输入框宽度 */
	width: 250px;
	/* 输入框高度 */
	height: 40px;
	/* 字体 */
	font-family: Arial, Helvetica, sans-serif;
	/* 字体大小 */
	font-size: 14px;
	/* 内边距 */
	padding: 5px;
	/* 边框圆角 */
	border-radius: 4px;
}

/* 输入框样式 */
input[type='text'] {
	/* 输入框宽度 */
	width: 10%;
	min-width: 100px;
	/* 输入框高度 */
	height: 5%;
	min-height: 20px;
	/* 字体 */
	font-family: Arial, Helvetica, sans-serif;
	/* 字体大小 */
	font-size: 14px;
	/* 内边距 */
	padding: 5px;
	/* 边框样式 */
	border: 5px solid #ccc;
	/* 边框圆角 */
	border-radius: 4px;
}

/* 输入框的placeholder样式 */
input[type='text']::placeholder {
	color: #999;
	/* 文字颜色 */
}

/* 控制台文本 */
.console {
	/* 保留空格、自动换行 */
	white-space: pre-wrap;
	/* 字体 */
	font-family: Consolas, Monaco, 'Courier New', monospace;
	font-size: larger;
	font-weight: inherit;
	/* 固定尺寸 */
	height: 100px;
	/* 隐藏垂直滚动条 */
	overflow-y: hidden;
	/* 隐藏水平滚动条 */
	overflow-x: hidden;
	/* 隐藏尺寸调整 */
	resize: both;
	/* 滚动行为 */
	scroll-behavior: auto;
	/* 背景颜色 */
	background-color: #222;
	/* 文字颜色 */
	color: #fff;
}
</style>
