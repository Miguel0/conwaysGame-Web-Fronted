<template>
  <generic-modal>
    <slot name='header'>Create Game</slot>

    <slot name='body'>

      <div class="form-group" v-bind:class="{ 'form-group--error': $v.name.$error }">
        <label class="form__label">Name</label>
        <input class="form__input" v-model.trim="name" @input="$v.name.$touch()">
      </div>
      <span class="form-group__message" v-if="!$v.name.required">Field is required</span>
      <span class="form-group__message" v-if="!$v.name.minLength">Name must have at least {{$v.name.$params.minLength.min}} letters.</span>
      
      <div class="form-group" v-bind:class="{ 'form-group--error': $v.refreshInterval.$error }">
        <label class="form__label">Refresh Interval (in milliseconds)</label>
        <input class="form__input" v-model="refreshInterval" @input="$v.refreshInterval.$touch()">
      </div>
      <span class="form-group__message" v-if="!$v.refreshInterval.between">Must be between {{$v.refreshInterval.$params.between.min}} and {{$v.refreshInterval.$params.between.max}}</span>

      <div class="form-group" v-bind:class="{ 'form-group--error': $v.color.$error }">
        <label class="form__label">User Color</label>
        <color-picker v-model="color" @change-color="onChangeColor"/>
      </div>
      <span class="form-group__message" v-if="!$v.color.required">Field is required</span>

    </slot>

    <slot name='footer'>

      <button class='modal-default-button' @click.prevent='$emit("close")'>Cancel</button>
      <button class='modal-default-button' @click.prevent='propagateNewGameInitialData'>Create Game</button>

    </slot>
  </generic-modal>
</template>

<script>
import { required, minLength, between } from 'vuelidate/lib/validators'
import { Compact } from 'vue-color'

export default {
  name: 'create-game-modal',
  components: {
    'color-picker': Compact
  },
  methods: {
    propagateNewGameInitialData () {
      this.$emit('createGame', this.$data)
    },
    onChangeColor (colorSelected) {
      this.$data.color = colorSelected
    }
  },
  validations: {
    name: {
      required,
      minLength: minLength(4)
    },
    refreshInterval: {
      between: between(500, 10000)
    },
    color: {
      required
    }
  },
  data () {
    return {
      name: '',
      refreshInterval: null,
      color: {
        hex: '#194d33',
        hsl: {
          h: 150,
          s: 0.5,
          l: 0.2,
          a: 1
        },
        hsv: {
          h: 150,
          s: 0.66,
          v: 0.30,
          a: 1
        },
        rgba: {
          r: 25,
          g: 77,
          b: 51,
          a: 1
        },
        a: 1
      }
    }
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
.form-group--error+.form-group__message {
    display: block;
    color: #f57f6c;
}

.form-group__message {
    font-size: .75rem;
    line-height: 1;
    display: none;
    margin-left: 14px;
    margin-top: -1.6875rem;
    margin-bottom: .9375rem;
}
</style>