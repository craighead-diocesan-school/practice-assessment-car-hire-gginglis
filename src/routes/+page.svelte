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
  // starts the cart as an empty array
  let totalCost = 0;
  // starts the cart's total cost as $0
  function calcTotalCost() {
    totalCost = 0;
    for (let car of cart) {
      totalCost = totalCost + car.price;
    }
  }
  const maxLength = 3;
  // prevents the user from adding more than 3 cars to their cart
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
      // takes away the price of the removed car from the the total cost
    }
    cart = [...cart.slice(0, index), ...cart.slice(index + 1)];
    // removes the car from the cart
  }
</script>

<head>
  <style>
    @import url("https://fonts.googleapis.com/css2?family=Audiowide&family=Raleway:ital,wght@0,100..900;1,100..900&display=swap");
  </style>
</head>

<Header />

<main>
  <div class="columns">
    <div class="column">
      <h2 class="title">Available Cars</h2>
      {#await cars}
        loading...
        <!-- displays a message while the array is being imported -->
      {:then cars}
        {#each cars.fast as car}
          <!-- shows each car as a seperate object -->
          {#if car.available}
            <!-- hides the unavailable cars -->
            <Car
              car={car.car}
              description={car.description}
              price={car.price}
              img={car.img}
              credit={car.credit}
              available={car.available}
            />
            <button
              on:click={() => addToCart(car)}
              disabled={cart.length >= maxLength}>add to cart</button
            >
            <!-- adds the car to the cart array and blocks the user from exceeding the maximum length of 3 cars -->
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
            <button
              on:click={() => addToCart(car)}
              disabled={cart.length >= maxLength}>add to cart</button
            >
          {/if}
        {/each}
      {/await}
    </div>

    <div class="column">
      <h2 class="title">Cart</h2>
      <h3>${totalCost}</h3>
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
            }}>remove from cart</button
          >
          <!-- removes the car from the cart array -->
        {/each}
      {:else}
        <p>your cart is empty</p>
        <!-- displays a message when the cart array has no items -->
      {/if}
    </div>
  </div>
</main>

<footer>
  <p>&copy; Craighead Diocesan School 2024</p>
</footer>

<style>
  button {
    color: #171d23;
    background-color: #171d234b;
    border: solid #171d23 1.5px;
    border-radius: 2px;
    font-size: 1rem;
    font-family: "Raleway", sans-serif;
    font-weight: 600;
    margin: 10px 10px 30px;
    transition: 0.25s;
  }
  h3 {
    color: #171d23;
    font-weight: bold;
    font-size: 1.5rem;
    font-weight: 600;
  }
  h2 {
    color: #171d23;
    font-family: "Audiowide", sans-serif;
    margin: 20px 10px;
    font-weight: 500;
    font-size: 1.7rem;
  }
  .column {
    margin: 10px;
  }
  p {
    color: #171d2380;
    font-family: "Raleway", sans-serif;
    font-weight: 500;
  }
  button:hover {
    background-color: #171d237f;
  }

  button:disabled {
    color: #171d239a;
    background-color: #171d2320;
    border: solid #171d239a 1.5px;
    transition: 0.25s;
  }
</style>
