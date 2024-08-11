<script>
  import Header from "$lib/Header.svelte";
  import Car from "$lib/Car.svelte";

  async function getCars() {
    let shopData = await fetch(
      "https://digitech.craighead.school.nz/api/car-hire",
    );

    return shopData.json();
  }

  let cars = getCars();
</script>

<Header />

<main>
  {#await cars}
    waiting...
  {:then cars}
    {#each cars.fast as car}
      {#if car.available}
        <Car
          car={car.car}
          description={car.description}
          price={car.price}
          img={car.img}
          credit={car.credit}
          available={car.available}
        />
      {/if}
    {/each}
    {#each cars.nice as car}
      {#if car.available}
        <Car
          car={car.car}
          description={car.description}
          price={car.price}
          img={car.img}
          credit={car.credit}
          available={car.available}
        />
      {/if}
    {/each}
  {/await}
</main>

<footer>
  <p>&copy; Craighead Diocesan School 2024</p>
</footer>
