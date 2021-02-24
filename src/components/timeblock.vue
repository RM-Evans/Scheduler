<template>
  <div class="block">
    <div class="time">
        {{ time.toFormat("hh a") }}
    </div>

    <div class="text">
        <textarea
          v-model="title"
          ref="textarea"
          :class="{ 'grey': isNotPassed, 'green': isPassed, 'red': isCurrent }"
        >
        </textarea>
    </div>

    <div class="save" @click="save()">
        <p>Save</p>
    </div>
  </div>
</template>

<script>
import { computed, ref } from 'vue';

export default {
  props: ['unit', 'current'],
  setup(props) {
    const textarea = ref(null);
    const time = computed(() => props.current.set({ hour: props.unit, minute: 0, second: 0 }));
    const title = ref(localStorage[`${props.unit}`] ?? '');

    // our condtional values, for our style binding
    const isPassed = computed(() => time.value.toMillis() < props.current.toMillis());
    const isCurrent = computed(() => time.value.hour === props.current.hour);
    const isNotPassed = computed(() => time.value.toMillis() > props.current.toMillis());

    const save = () => {
      // seems odd, but we need to access our ref value which gives us a...
      // html element, then text area's element value
      localStorage[`${props.unit}`] = textarea.value.value;
    };

    return {
      textarea, time, title, isPassed, isNotPassed, isCurrent, save,
    };
  },
};
</script>

<style lang="scss" scoped>

    .grey { background: #CDCACE; }
    .green { background: #74E96E; }
    .red { background: #F14D57; }

    .block {
        height: 100px;
        display: grid;
        grid-template-columns: 1fr 5fr 1fr;

        .time, .save {
            width: 100%;
            height: 100%;
            display: grid;
            place-content: center;
        }

        .time {
            border-top: 5px dashed black;
        }

        .save {
            color: white;
            background: #12AFC9;
            border-radius: 0 15px 15px 0;
            transition: color 150ms ease-in-out;
            -webkit-user-select: none;

            &:hover {
                cursor: pointer;
                color: black;
            }
        }

        .text textarea {
            width: 100%;
            // overide the height value, because it's misses 1 px,
            // when you get rid of the border for some reason...
            height: calc(100% + 1px) !important;
            resize: none;
            border: none;
            font-size: 1.1em;
            padding: 20px;
            // stops the padding from shifitng text box outside of it's container
            box-sizing: border-box;
            font-weight: bold;

            &:focus {
                border: none;
                outline: none;
            }
        }
    }
</style>
