<template>
  <div class="c-checkbox">
    <input type="checkbox" class="c-checkbox__control" v-model="checkboxModel" :id="id" @focus="isFocused = true" @blur="isFocused = false" ref="hiddenInput">
    <span class="c-checkbox__icon" @click="toggle()" role="checkbox" :aria-checked="isChecked" :aria-labelledby="labelId" tid="c-checkbox__icon"
          :class="{ 'c-checkbox__icon--checked': isChecked, 'c-checkbox__icon--unchecked': !isChecked, 'c-checkbox__icon--focused': isFocused }"></span>
    <label :id="labelId" class="c-checkbox__label" :for="id" tid="c-checkbox__label"><slot></slot></label>
  </div>
</template>
<script>
import Vue from 'vue';
import { Component, Model, Prop } from 'vue-property-decorator';
import uniqueId from 'lodash/uniqueId';

@Component
export default class MyCheckbox extends Vue {
  @Model('change') @Prop(Boolean) value;

  id = uniqueId('my-checkbox-');
  labelId = `${this.id}-label`;
  isFocused = false;

  get checkboxModel () {
    return this.value;
  }

  set checkboxModel (value) {
    this.$emit('change', value);
  }

  get isChecked () {
    return !!this.checkboxModel;
  }

  toggle () {
    this.checkboxModel = !this.checkboxModel;
    this.$refs.hiddenInput.focus();
  }
}
</script>
<style>
.c-checkbox {
  display: flex;
  flex-direction: row;
}

.c-checkbox__control {
  position: absolute !important;
  height: 1px;
  width: 1px;
  overflow: hidden;
  clip: rect(1px, 1px, 1px, 1px);
}

.c-checkbox__icon {
  width: 2.5rem;
  height: 2.5rem;
  line-height: 2.5rem;
  text-align: center;
  border: 0.1rem solid #ccc;
  border-radius: 0.4rem;
  cursor: pointer;
}

.c-checkbox__icon--checked:before {
  content: "✓";
  color: rgb(2, 116, 169);
}

.c-checkbox__icon--focused {
  border-color: rgb(2, 116, 169);
}

.c-checkbox__label:not(:empty) {
  margin-left: 1.5rem;
  line-height: 2.5rem;
}
</style>
