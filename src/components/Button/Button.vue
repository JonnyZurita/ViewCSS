<template>
  <component
    :is="tagName"
    :class="classes"
    @click="handleClickLink" 
    v-bind="tagProps"
  >
    <span 
      v-if="showSlot" 
      ref="slot"
    >
      <slot></slot>
    </span>
  </component>
</template>

<script>
  import { oneOf } from '../../utils/assist';
  // Prefix
  const prefixCls = 'button';

  export default {
    name: 'Button',
    props: {
      type: {
        validator (value) {
          return oneOf(value, ['primary','secondary', 'danger','info', 'success', 'warning']);
        },
      },
      round: {
        validator (value) {
          return oneOf(value, ['circle', 'round']);
        }
      },
      size: {
        validator (value) {
          return oneOf(value, ['mini', 'small', 'large']);
        },
      },
      loading: Boolean,
      disabled: Boolean,
      htmlType: {
        default: 'button',
        validator (value) {
          return oneOf(value, ['button', 'submit', 'reset']);
        }
      },
      icon: {
        type: String,
        default: ''
      },
      customIcon: {
        type: String,
        default: ''
      },
      long: {
        type: Boolean,
        default: false
      },
      outline: {
        type: Boolean,
        default: false
      }
    },
    data () {
      return {
        showSlot: true
      };
    },
    computed: {
      classes () {
        return [
          `${prefixCls}`,
          `${prefixCls}--${this.type}`,
          `${prefixCls}--${this.size}`,
          `${prefixCls}--${this.round}`,
          `${prefixCls}--${this.outline}`,
          {
            [`${prefixCls}--long`]: this.long,
            [`${prefixCls}-loading`]: this.loading,
            [`${prefixCls}-icon-only`]: !this.showSlot && (!!this.icon || !!this.customIcon || this.loading),
          }
        ];
      },
      // Point out if it should render as <a> tag
      isHrefPattern() {
        const {to} = this;
        return !!to;
      },
      tagName() {
        const {isHrefPattern} = this;
        return isHrefPattern ? 'a' : 'button';
      },
      tagProps() {
        const {isHrefPattern} = this;
        if(isHrefPattern) {
          const {linkUrl,target}=this;
          return {href: linkUrl, target};
        } else {
          const {htmlType} = this;
          return {type: htmlType};
        }
      }
    },
    methods: {
      // Ctrl or CMD and click, open in new window when use `to`
      handleClickLink (event) {
        this.$emit('click', event);
        const openInNewWindow = event.ctrlKey || event.metaKey;
        this.handleCheckClick(event, openInNewWindow);
      }
    },
    mounted () {
      this.showSlot = this.$slots.default !== undefined;
    }
  }
</script>
