<script setup>
import { ref, reactive, computed } from 'vue';

const props = defineProps({
  color: {
    type: String,
    default: '#ab92e9',
    validator: (value) =>
      typeof value === 'string' &&
      (value.startsWith('#') || /^[a-z]+$/i.test(value)),
  },
  size: {
    type: [String, Number],
    default: 'medium',
    validator: (value) =>
      ['small', 'medium', 'large'].includes(value) || typeof value === 'number',
  },
  shape: {
    type: String,
    default: 'rectangle',
    validator: (value) => ['rectangle', 'rounded', 'circle'].includes(value),
  },
  isDisabled: {
    type: Boolean,
    default: false,
  },
  isLoading: {
    type: Boolean,
    default: false,
  },
});

let isHover = ref(false);
let isActive = ref(false);

// font based on bgcolor
const isDark = (color) => {
  const c = color.substring(1);
  const rgb = parseInt(c, 16);
  const r = (rgb >> 16) & 0xff;
  const g = (rgb >> 8) & 0xff;
  const b = (rgb >> 0) & 0xff;
  const luma = 0.2126 * r + 0.7152 * g + 0.0722 * b;
  return luma < 128;
};

const btnStyles = computed(() => {
  let obj = {
    fontSize: '',
    backgroundColor: props.color,
    color: isDark(props.color) ? 'white' : 'black',
  };

  if (typeof props.size === 'number') {
    obj.fontSize = `${props.size}px`;
    return obj;
  }
  const fontSizeMap = {
    small: '14px',
    medium: '20px',
    large: '28px',
  };
  obj.fontSize = fontSizeMap[props.size];
  return obj;
});

const btnClasses = computed(() => {
  return {
    [props.shape]: true,
    [props.size]: true,
    // [props.color]: true,
    'is-hover': isHover.value,
    'is-active': isActive.value,
    'is-inactive': props.isDisabled,
  };
});

const emit = defineEmits([
  'click',
  'mouseover',
  'mouseout',
  'mousedown',
  'mouseup',
]);

const handleClick = (evt) => {
  if (!props.isDisabled) {
    isActive.value = true;
    emit('click', evt);
  }
};
const onMouseOver = (evt) => {
  if (!props.isDisabled) {
    isHover.value = true;
    emit('mouseover', evt);
  }
};
const onMouseOut = (evt) => {
  if (!props.isDisabled) {
    isHover.value = false;
    isActive.value = false;
    emit('mouseout', evt);
  }
};
const onMouseDown = (evt) => {
  if (!props.isDisabled) {
    isActive.value = true;
    emit('mousedown', evt);
  }
};
const onMouseUp = (evt) => {
  if (!props.isDisabled) {
    isActive.value = false;
    emit('mouseup', evt);
  }
};
</script>

<template>
  <button
    :class="btnClasses"
    :style="btnStyles"
    :disabled="props.isDisabled || props.isLoading"
    @mouseover="onMouseOver"
    @mouseout="onMouseOut"
    @mousedown="onMouseDown"
    @mouseup="onMouseUp"
    @click="handleClick"
  >
    <slot class="btn-content"> </slot>
    <span v-if="props.isLoading" class="is-loading"></span>
  </button>
</template>

<style scoped>
/* base button styles */
button {
  cursor: pointer;
  border: 1px solid;
  outline: none;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  transition: all 0.3s ease;
  padding: 10px 20px;
  font-weight: bold;
  margin: 0px auto;
  transition: background-color 0.3s, transform 0.3s;
  box-shadow: 0 14px 16px rgba(0, 0, 0, 0.5);
  border-radius: 2px;
}
/* shape related style start */
.rectangle {
  /* border-radius: 0; */
}
.rounded {
  border-radius: 10px;
}
.circle {
  border-radius: 50%;
  padding: 15px;
}
/* shape related style end */
.is-hover:hover {
  -webkit-transform: scale(1.1);
  -ms-transform: scale(1.1);
  transform: scale(1.1);
}
.is-active:not(.is-inactive) {
  animation: click 0.1s ease-out;
  transform: scale(1.06);
  border: 2px dotted;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.is-inactive,
button:has(> span.is-loading) {
  /* Inactive state styles */
  opacity: 0.5;
  cursor: not-allowed !important;
  background-color: #cccccc !important;
  color: #666666 !important;
  box-shadow: none !important;
}

.is-loading {
  display: inline-block;
  margin-left: 8px;
  width: 15px;
  height: 15px;
  border: 3px solid #fff;
  border-top-color: v-bind(props.color);
  border-radius: 50%;
  animation: spin 0.6s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
.small {
  font-size: 14px;
}
.medium {
  font-size: 20px;
}
.large {
  font-size: 28px;
}

@keyframes click {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(1px);
  }
  100% {
    transform: translateY(0);
  }
}
</style>
