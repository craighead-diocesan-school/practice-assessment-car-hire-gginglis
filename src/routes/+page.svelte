<script>
  import Header from "$lib/Header.svelte";
  import Car from "$lib/Car.svelte";

  async function getCars() {
    let shopData = await fetch(
      "https://digitech.craighead.school.nz/api/car-hire",
    );
    // collects the array from the external source

    return shopData.json();
  }

  let cars = getCars();
  //

  let cart = [];

  let totalCost = 0;

  function calcTotalCost() {
    totalCost = 0;
    for (let car of cart) {
      totalCost = totalCost + car.price;
    }
  }
  const maxLength = 3;

  function addToCart(car) {
    if (cart.length < maxLength) {
      cart = [...cart, car];
      calcTotalCost();
    }
  }

  function removeFromCart(index) {
    const car = cart[index];
    if (car) {
      totalCost = totalCost - car.price;
    }
    cart = [...cart.slice(0, index), ...cart.slice(index + 1)];
  }
</script>

<Header />

<main>
  <div class="columns">
    <div class="column">
      <h2 class="title">Available Cars</h2>
      {#await cars}
        waiting...
      {:then cars}
        {#each cars.fast as car}
          {#if car.available}
            <button
              on:click={() => addToCart(car)}
              disabled={cart.length >= maxLength}>add to cart</button
            >
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
            <button
              on:click={() => addToCart(car)}
              disabled={cart.length >= maxLength}>add to cart</button
            >
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
    </div>

    <div class="column">
      <h2 class="title">Cart ${totalCost}</h2>
      {#if cart.length >= 1}
        {#each cart as car, index}
          <p>
            <Car
              car={car.car}
              description={car.description}
              price={car.price}
              img={car.img}
              credit={car.credit}
              available={car.available}
            />
          </p>
          <button
            on:click={() => {
              removeFromCart(index);
            }}>-</button
          >
        {/each}
      {:else}
        <h3>your cart is empty</h3>
      {/if}
    </div>
  </div>
</main>

<footer>
  <p>&copy; Craighead Diocesan School 2024</p>
</footer>
