<script>
  import Rate from "../../Rate.svelte";

  const beforeRate = rate => {
    console.log(rate);
  };
  const afterRate = rate => {
    console.log(rate);
  };
</script>

<p>Rate</p>

<Rate
  {beforeRate}
  {afterRate}
  length={5}
  ratedesc={['Very bad', 'bad', 'Normal', 'Good', 'Very good']}
  showCount={true} />
