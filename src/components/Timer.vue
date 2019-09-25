<template>
	<div class="timer">
		<div class="clock">
			{{minutes}}:{{seconds}}
		</div>
		<div class="controls">
			<div>
				<button :disabled="isRunning" @click="start">start</button>
				<button :disabled="!isRunning" @click="pause">pause</button>
			</div>
			<div>
				<button @click="setPomodoro">set pomodoro</button>
				<button @click="setBreak">set break</button>
			</div>
		</div>
	</div>
</template>

<script>
const pomodoroLength = 25*60;
const breakLength = 5*60;

function _zeropad(number) {
	const str = String(number);
	return str.padStart(2, '0');
}

export default {
	data () {
		return {
			totalSeconds: 0,
			intervalId: null,
		}
	},
	computed: {
		isRunning: vm => !!vm.intervalId,
		minutes: vm => parseInt(vm.totalSeconds/60, 10),
		seconds: vm => _zeropad(vm.totalSeconds%60),
	},
	created () {
		this.setPomodoro()
	},
	methods: {
		setPomodoro () {
			this.pause()
			this.totalSeconds = pomodoroLength
		},
		setBreak () {
			this.pause()
			this.totalSeconds = breakLength
		},
		start () {
			if (this.intervalId) return

			this.intervalId = setInterval(() => {
				this.countdown()
			}, 1000)
		},
		pause () {
			clearInterval(this.intervalId)
			this.intervalId = null
		},
		countdown () {
			this.totalSeconds--

			if (this.totalSeconds <= 0) {
				this.totalSeconds = 0
				this.pause()
				this.alarm()
			}
		},
		alarm () {
			// TODO: implement alarm
		},
	},
}
</script>

<style>
.clock {
	font-size: 48pt;
}
.controls button {
	font-size: 16pt;
	background: #eee;
	border: #ccc 1px solid;
	border-radius: 8px;
	padding: 8px 16px;
	margin: 4px;
	width: 200px;
}
</style>
