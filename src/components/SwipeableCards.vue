<template>
  <section class="container">
    <div class="fixed header">
      <i class="material-icons" @click="index = 0">refresh</i>
      <!-- <span>Kittynder</span> -->
      <img class="mct-logo" :src="require(`../assets/images/mct.png`)" width="40" height="60" />
      
      <i class="material-icons" @click="match">favorite</i>
    </div>
    <div
      v-if="current"
      class="fixed fixed--center"
      style="z-index: 3"
      :class="{ 'transition': isVisible }">
      <Vue2InteractDraggable
        v-if="isVisible"
        :interact-out-of-sight-x-coordinate="300"
        :interact-max-rotation="15"
        :interact-x-threshold="100"
        :interact-y-threshold="100"
        :interact-event-bus-events="interactEventBus"
        interact-lock-y-axis="true"
        interact-block-drag-down="true"
        @draggedRight="emitAndNext('match')"
        @draggedLeft="emitAndNext('reject')"
        @draggedUp="emitAndNext('skip')"
        class="rounded-borders card card--one">
        <div style="height: 100%">
          <img
            :src="require(`../assets/images/${current.src}`)"
            class="rounded-borders"/>
          <div class="text">
            <!-- <h2>{{current.name}}, <span>{{current.age}}</span></h2> -->
            <h2>{{current.name}}</h2>
          </div>
        </div>
      </Vue2InteractDraggable>
    </div>
    <div
      v-if="next"
      class="rounded-borders card card--two fixed fixed--center"
      style="z-index: 2">
      <div style="height: 100%">
        <img
          :src="require(`../assets/images/${next.src}`)"
          class="rounded-borders"/>
        <div class="text">
            <!-- <h2>{{next.name}}, <span>{{next.age}}</span></h2> -->
            <h2>{{next.name}}</h2>
          </div>
      </div>
    </div>
    <div
      class="rounded-borders card card--three fixed fixed--center"
      style="z-index: 1">
      <div style="height: 100%" class="restart-card">
        <h2>Thanks for following along!</h2>
        <h2>Restart?</h2>
        <div class="btn btn--skip" @click="index = 0">
          <i class="material-icons">refresh</i>
        </div>
      </div>
    </div>
    <div class="footer fixed">
      <div class="btn btn--decline" @click="reject">
          <i class="material-icons">close</i>
      </div>
      <!-- <div class="btn btn--skip" @click="skip">
          <i class="material-icons">call_missed</i>
      </div> -->
      <div class="btn btn--like" @click="match">
          <i class="material-icons">favorite</i>
      </div>
    </div>
  </section>
</template>
<script>
import { Vue2InteractDraggable, InteractEventBus } from 'vue2-interact'
const EVENTS = {
  MATCH: 'match',
  SKIP: 'skip',
  REJECT: 'reject'
}

export default {
  name: 'SwipeableCards',
  components: { Vue2InteractDraggable },
  data() {
    return {
      isVisible: true,
      index: 0,
      interactEventBus: {
        draggedRight: EVENTS.MATCH,
        draggedLeft: EVENTS.REJECT,
        draggedUp: EVENTS.SKIP
      },
      cards: [
        { src: 'brian-mike.jpg', name: 'Mike "Brian" Cox', age: 5 },
        { src: 'eddie-mike.jpg', name: 'Mike "Eddie" Redmayne', age: 7 },
        { src: 'cute-mike.jpg', name: 'Cute Mike', age: 3 },
        { src: 'learner-mike.jpg', name: 'Learner Mike', age: 9 },
        { src: 'upside-down-mike.jpg', name: 'Upside Down Mike', age: 8 },
        { src: 'prison-mike.jpg', name: 'Prison Mike', age: 6 },
        { src: 'school-mike.jpg', name: 'School Mike', age: 3 },
        { src: 'smooth-mike.jpg', name: 'Smooth Mike', age: 7 },
        { src: 'anti-antisocial-mike.jpg', name: 'Anti anti-social Mike', age: 7 },
        { src: 'fike-meng.jpg', name: 'Mike & Feng <3', age: 4 },
      ]
    }
  },
  computed: {
    current() {
      return this.cards[this.index]
    },
    next() {
      return this.cards[this.index + 1]
    }
  },
  methods: {
    match() {
      InteractEventBus.$emit(EVENTS.MATCH)
    },
    reject() {
      InteractEventBus.$emit(EVENTS.REJECT)
    },
    skip() {
      InteractEventBus.$emit(EVENTS.SKIP)
    },
    emitAndNext(event) {
      this.$emit(event, this.index)
      setTimeout(() => this.isVisible = false, 200)
      setTimeout(() => {
        this.index++
        this.isVisible = true
      }, 200)
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  background: #eceff1;
  width: 100%;
  height: 100vh;
}

.header {
  width: 100%;
  height: 60vh;
  z-index: 0;
  top: 0;
  left: 0;
  color: white;
  text-align: center;
  font-style: italic;
  font-family: 'Engagement', cursive;
  background: #5388f9;
  background: -webkit-linear-gradient(to top, #0a0767, #5388f9);
  background: linear-gradient(to top, #0a0767, #5388f9);
  clip-path: polygon(0 1%, 100% 0%, 100% 76%, 0 89%);
  display: flex;
  justify-content: space-between;
  span {
    display: block;
    font-size: 4rem;
    padding-top: 2rem;
    text-shadow: 1px 1px 1px red;
  }
  i {
    padding: 24px;
  }
}

.mct-logo {
  animation: rotation 4s infinite linear;
  margin-top: 10px;
  width: 60px;
  height: 80px;
}

@keyframes rotation {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(359deg);
  }
}

.restart-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
  text-align: center;
}

.footer {
  width: 77vw;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  padding-bottom: 30px;
  justify-content: space-around;
  align-items: center;
}

.btn {
  position: relative;
  width: 50px;
  height: 50px;
  padding: .2rem;
  border-radius: 50%;
  background-color: white;
  box-shadow: 0 6px 6px -3px rgba(0,0,0,0.02), 0 10px 14px 1px rgba(0,0,0,0.02), 0 4px 18px 3px rgba(0,0,0,0.02);
  cursor: pointer;
  transition: all .3s ease;
  user-select: none;
  -webkit-tap-highlight-color:transparent;
  &:active {
    transform: translateY(4px);
  }
  i {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    &::before {
      content: '';
    }
  }
  &--like {
    background-color: red;
    padding: .5rem;
    color: white;
    box-shadow: 0 10px 13px -6px rgba(0,0,0,.2), 0 20px 31px 3px rgba(0,0,0,.14), 0 8px 38px 7px rgba(0,0,0,.12);
    i {
      font-size: 32px;
    }
  }
  &--decline {
    color: red;
  }
  &--skip {
    color: green;
  }
}

.flex {
  display: flex;
  &--center {
    align-items: center;
    justify-content: center;
  }
}

.fixed {
  position: fixed;
  &--center {
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
}
.rounded-borders {
  border-radius: 12px;
}
.card {
  width: 80vw;
  height: 60vh;
  color: white;
  img {
    object-fit: cover;
    display: block;
    width: 100%;
    height: 100%;
  }
  &--one {
    box-shadow: 0 1px 3px rgba(0,0,0,.2), 0 1px 1px rgba(0,0,0,.14), 0 2px 1px -1px rgba(0,0,0,.12);
  }
  &--two {
    transform: translate(-48%, -48%);
    box-shadow: 0 6px 6px -3px rgba(0,0,0,.2), 0 10px 14px 1px rgba(0,0,0,.14), 0 4px 18px 3px rgba(0,0,0,.12);
  }
  &--three {
    background: rgba(black, .8);
    transform: translate(-46%, -46%);
    box-shadow: 0 10px 13px -6px rgba(0,0,0,.2), 0 20px 31px 3px rgba(0,0,0,.14), 0 8px 38px 7px rgba(0,0,0,.12);
  }
  .text {
    position: absolute;
    bottom: 0;
    width: 100%;
    background:black;
    background:rgba(0,0,0,0.5);
    border-bottom-right-radius: 12px;
    border-bottom-left-radius: 12px;
    text-indent: 20px;

    h2 {
      font-size: 1.2em;
    }
    span {
      font-weight: normal;
    }
  }
}

.transition {
  animation: appear 200ms ease-in;
}

@keyframes appear {
  from {
    transform: translate(-48%, -48%);
  }
  to {
    transform: translate(-50%, -50%);
  }
}
</style>
