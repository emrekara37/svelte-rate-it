<script>
  import { onMount, beforeUpdate } from "svelte";

  export let value = 0;
  export let name = "rate";
  export let length = 5;
  export let showCount=false;
  export let required = false;
  export let ratedesc = [];
  export let beforeRate;
  export let afterRate;
  export let disabled = false;
  export let readonly = false;

  let arr = [];
  let rate = 0;
  let over = 0;
  $: if (value) {
    rate = convertValue(value);
    over = convertValue(value);
  }

  const convertValue = val => {
    if (val >= length) {
      val = length;
    } else if (val < 0) {
      val = 0;
    }
    return val;
  };
  const onOver = index => {
    if (!readonly) over = index;
  };
  const onOut = () => {
    if (!readonly) over = rate;
  };
  const setRate = index => {
    if (readonly) return false;
    if (typeof beforeRate === "function") {
      beforeRate(rate);
    }
    rate = index;
    if (typeof afterRate === "function") {
      afterRate(rate);
    }
  };
  const isFilled = index => {
    return index <= over;
  };
  const isEmpty = index => {
    return index > over || (!value && !over);
  };

  const createArray = () => {
    for (let i = 1; i <= length; i++) {
      arr.push(i);
    }
  };
  beforeUpdate(() => {
    if (arr.length === 0) {
      createArray();
    }
  });
  onMount(() => {
    value = convertValue(value);
    rate = convertValue(value);
    over = convertValue(value);
  });
</script>

<style scoped>
  /*author : @SinanMtl  */
  .icon {
    display: inline-block;
    width: 16px;
    height: 16px;
    stroke-width: 0;
    stroke: currentColor;
    fill: currentColor;
    vertical-align: middle;
    top: -2px;
    position: relative;
    margin: 0 5px;
  }
  .Rate {
    cursor: default;
  }
  .Rate__star {
    color: #dedbdb;
    display: inline-block;
    padding: 7px;
    text-decoration: none;
    cursor: pointer;
    background: transparent none;
    border: 0;
  }
  .Rate__star .icon {
    top: 0;
    vertical-align: middle;
  }
  .Rate__star.hover {
    color: #efc20f;
  }
  .Rate__star.filled {
    color: #efc20f;
  }
  .Rate__star:hover,
  .Rate__star:focus {
    text-decoration: none;
  }
  .Rate__view .count,
  .Rate__view .desc {
    display: inline-block;
    vertical-align: middle;
    padding: 7px;
  }
  .Rate.has-error .Rate__star {
    color: #f37a77;
  }
  .Rate.has-error .Rate__star.hover {
    color: #efc20f;
  }
  .Rate.has-error .Rate__star.filled {
    color: #efc20f;
  }
  .Rate__star[disabled] {
    opacity: 0.8;
  }
  .Rate__star.hover[disabled],
  .Rate__star.filled[disabled] {
    color: #efc20f;
    opacity: 0.6;
  }
  .Rate__view.disabled .count,
  .Rate__view.disabled .desc {
    color: #ccc;
  }
</style>

{#if length > 0}
  <div class="Rate">
    <svg
      style="position: absolute; width: 0; height: 0;"
      width="0"
      height="0"
      version="1.1"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink">
      <defs>
        <symbol id="icon-star-full" viewBox="0 0 32 32">
          <title>star-full</title>
          <path
            d="M32 12.408l-11.056-1.607-4.944-10.018-4.944 10.018-11.056 1.607 8
            7.798-1.889 11.011 9.889-5.199 9.889 5.199-1.889-11.011 8-7.798z" />
        </symbol>
      </defs>
    </svg>
    <input type="hidden" {name} bind:value={rate} {required} />

    {#each arr as n}
      <button
        type="button"
        key={n}
        class:hover={n <= over}
        class:filled={n <= rate || isFilled(n)}
        class={'Rate__star'}
        on:mouseover={() => {
          onOver(n);
        }}
        on:mouseout={() => {
          onOut(n);
        }}
        on:click={() => {
          setRate(n);
        }}
        on:keyup={() => {
          onOver(n);
        }}
        on:keyup.enter={() => {
          setRate(n);
        }}
        {disabled}>
        <svg class="icon">
          <use xlink:href="#icon-star-full" />
        </svg>
      </button>
    {/each}
    <div class="Rate__view" class::disabled={disabled}>
      {#if showCount && over > 0}
        <span class="count">{over}</span>
      {/if}
      {#if ratedesc.length > 0 && over > 0}
        <span class="desc">{ratedesc[over - 1]}</span>
      {/if}
    </div>
  </div>
{/if}
