# Svelte Rate It

> Rate component for Svelte. Based on [vue-rate](https://github.com/SinanMtl/vue-rate).
Thanks [@SinanMtl](https://github.com/SinanMtl)

## Installation and usage

Install rate component for your project

```bash
npm install svelte-rate-it --save
```
or with yarn
```bash
yarn add svelte-rate-it
```


Import Svelte Rate into your app

```javascript
import Rate from "svelte-rate-it/Rate.svelte";

```

Use HTML template

```html
<Rate length={5} />
```

## Options from props

- `length {number}`: Star size

```html
<Rate length={5} />
```

- `value {number}`: Default value

```html
<Rate length={5} value={2} />
```

- `showCount {boolean}`: Shows rate number when mouseover the star.

```html
<Rate length={5} showCount={true} />
```

- `ratedesc {object}`: Rate star description array. 

```html
<Rate
  length={5}
  ratedesc={['Very bad', 'bad', 'Normal', 'Good', 'Very good']} />

```

- `disabled {boolean}`: Disable rate.

```html
<Rate length={5} value={2} disabled={true} />
```

- `readonly {boolean}`: Read-only rate.

```html
<rate length={5} value={2} readonly={true} />
```



## Events

```javascript
const beforeRate = rate => {
    console.log(rate);
  };
  const afterRate = rate => {
    console.log(rate);
  };
})
```

```html
<Rate
  {beforeRate}
  {afterRate}
  length={5}
  ratedesc={['Very bad', 'bad', 'Normal', 'Good', 'Very good']}
  showCount={true} />

```

