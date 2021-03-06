<template>
  <div class="checkbox">
    <label>
      <input type="checkbox" :name="keyName" v-model="checked" />
      <i class="md-checkbox-control"></i>
      {{ text }}
    </label>
  </div>
</template>

<script lang="ts">
  import Vue from 'vue';
  import { CheckBoxStates } from '~/interfaces/CheckBoxInterface';
  import EventBus from '~/components/reusable/EventBus';

  export default Vue.extend({
    props: {
      text: {
        type: String,
        required: true,
        default: '',
      },
      keyName: {
        type: String,
        required: true,
        default: '',
      },
    },
    data(): CheckBoxStates {
      return {
        checked: false,
      };
    },
    mounted() {
      EventBus.$on('uncheckAll', () => {
        this.checked = false;
      });
    },
    watch: {
      checked(newValue): void {
        this.$emit('checkboxUpdate', {
          keyName: this.keyName,
          name: this.keyName,
          title: this.text,
          checked: newValue,
        });
      },
    },
  });
</script>

<style lang="scss">
  $md-checkbox-margin: initial !default;
  $md-checkbox-checked-color: $primary !default;
  $md-checkbox-border-color: $primary !default;
  $md-checkbox-disabled-opacity: 0.5 !default;

  $md-checkbox-size: 20px !default;
  $md-checkbox-padding: 3px !default;

  $md-checkmark-width: 2px !default;
  $md-checkmark-color: $white !default;

  .checkbox {
    position: relative;
    margin: $md-checkbox-margin;
    cursor: pointer;
    text-align: left;
    background: $white;
    width: 220px;
    border-radius: 3px;
    margin: 5px 15px;
    box-shadow: 0 0 0 0 rgba(255, 255, 255, 0.6);
    transition: 0.25s ease-out;

    &:hover {
      box-shadow: 0 0 10px 0px rgba(255, 255, 255, 0.6);
    }

    &.md-checkbox-inline {
      display: inline-block;
    }

    label {
      padding: 8px;
      padding-left: $md-checkbox-size + 25px;
      margin-bottom: 0;
      display: flex;
      align-items: center;
      cursor: pointer;
      font-weight: 600;
    }

    .md-checkbox-control {
      cursor: pointer;
      position: absolute;
      top: 25%;
      transform: translateY(-50%);
      left: 10px;

      &::before,
      &::after {
        position: absolute;
        top: 0;
        left: 0;
        content: '';
      }

      &::before {
        // box
        width: $md-checkbox-size;
        height: $md-checkbox-size;
        cursor: pointer;
        background: #fff;
        border: 2px solid $md-checkbox-border-color;
        border-radius: 2px;
        transition: background 0.3s;
      }
    }

    [type='checkbox'] {
      display: none;
      outline: 0;

      &:disabled {
        + .md-checkbox-control {
          cursor: not-allowed;
          opacity: $md-checkbox-disabled-opacity;

          &::before {
            cursor: not-allowed;
          }
        }
      }
    }

    [type='checkbox']:checked,
    .indeterminate {
      + .md-checkbox-control {
        &::before {
          background: $md-checkbox-checked-color;
          border: none;
        }
        &::after {
          $md-checkmark-size: $md-checkbox-size - 2 * $md-checkbox-padding;

          /* stylelint-disable */
          top: ($md-checkbox-size / 2) - ($md-checkmark-size / 4) -
            $md-checkbox-size / 10;
          /* stylelint-enable */
          left: $md-checkbox-padding;
          width: $md-checkmark-size;
          height: $md-checkmark-size / 2;

          border: $md-checkmark-width solid $md-checkmark-color;
          border-top-style: none;
          border-right-style: none;

          transform: rotate(-45deg);
        }
      }
    }

    .indeterminate {
      + .md-checkbox-control {
        &::after {
          top: ($md-checkbox-size / 2) - ($md-checkmark-width / 2);
          height: 0;
          transform: rotate(0);
        }
      }
    }
  }
</style>
