<template>
    <p>{{ timer }}</p>
    <div class="char"></div>
</template>

<script>
export default {
    name: 'App',
    data() {
        return {
            animationDuration: 1800,
            jumpTime: Math.floor(Math.random() * 5000) + 1000,
            timer: 0,
            currentLeft: 0,
            currentTop: 0,
        };
    },
    computed: {
        formattedTime() {
            return Math.floor(this.jumpTime / 1000)
        },
    },
    methods: {
        startTimer() {
            const timerInterval = setInterval(() => {
                if (this.timer > 0) {
                    this.timer--;
                } else {
                    clearInterval(timerInterval);
                }
            }, 1000);
        },
        startAnimation() {
            const char = document.querySelector('.char');
            char.style.animationDuration = this.animationDuration + 'ms';
            char.style.animationPlayState = 'running';
            const outcome = Math.floor(Math.random() * 3);
            setTimeout(() => {
                this.currentLeft = char.getBoundingClientRect().left;
                this.currentTop = char.getBoundingClientRect().top;
                switch (outcome) {
                    case 0:
                        this.startJumpAnimation();
                        break
                    case 1:
                        this.startFallAnimation();
                        break
                    case 2:
                        this.startFallAndDieAnimation();
                        break
                }
            }, this.jumpTime);
        },
        startJumpAnimation() {
            const char = document.querySelector('.char');
            char.style.left = this.currentLeft + 'px';
            char.style.animationPlayState = 'paused';
            char.style.animation = 'none';
            char.style.animation = 'jump 1s ease-out infinite';

        },
        startFallAnimation() {

            const char = document.querySelector('.char');
            char.style.left = this.currentLeft + 'px';
            char.style.animation = 'fall 1s ease-in-out';

        },
        startFallAndDieAnimation() {
            const char = document.querySelector('.char');
            char.style.left = this.currentLeft + 'px';
            char.style.animation = 'fall-and-dies 1s ease-in-out';
        },
    },

    mounted() {
        this.timer = this.formattedTime
        this.startTimer()
        this.startAnimation();
    },
};
</script>

<style>
.char {
    width: 130px;
    height: 130px;
    position: absolute;
    background: url("@/assets/spritesheet.png");
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    animation: dinos 1.8s steps(4) infinite, movedinos 4s linear infinite;
}

@keyframes dinos {
    from {
        background-position: 0;
    }
    to {
        background-position: 520px;
    }
}

@keyframes movedinos {
    0% {
        left: 0;
    }
    100% {
        left: 100%;
    }
}

@keyframes jump {
    0%, 100% {
        transform: translate(-50%, -50%);
    }
    20% {
        transform: translate(-50%, -100px);
    }
    80% {
        transform: translate(-50%, -100px);
    }
    100% {
        transform: translate(-50%, -50px);
    }
}

@keyframes fall {
    0%, 20% {
        transform: translate(-50%, -50%);
    }
    100% {
        transform: translate(-50%, 100px);
    }
}

@keyframes fall-and-dies {
    0% {
        transform: rotate(0deg);
    }
    25% {
        transform: rotate(180deg);
    }
    50% {
        transform: rotate(360deg);
    }
    100% {
        transform: rotate(540deg);
    }
}
</style>
