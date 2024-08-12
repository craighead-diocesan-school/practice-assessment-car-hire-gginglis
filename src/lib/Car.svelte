<script>
    export let car;
    export let description;
    export let price;
    export let img;
    export let credit;
    // import Cart from "$lib/Cart.svelte";
    // export let available;

    async function getCars() {
        let shopData = await fetch(
            "https://digitech.craighead.school.nz/api/car-hire",
        );
        return shopData.json();
    }

    // let cars = getCars();

    let cart = [];

    function addToCart(index) {
        cart = [...cart, index];
    }
</script>

<head>
    <style>
        @import url("https://fonts.googleapis.com/css2?family=Audiowide&display=swap");
    </style>
</head>

<main>
    <img src={img} alt="" title={credit} />
    <div class="info">
        <h2>{car}</h2>
        <h3>${price}</h3>
        <p>{description}</p>
    </div>
</main>

<style>
    main {
        display: flex;
    }
    .info {
        margin: 2px;
    }
    img {
        width: 400px;
    }

    h2 {
        color: #131415;
        font-family: "Audiowide", sans-serif;
        font-weight: 500;
        font-size: 1.6rem;
    }

    h3 {
        color: #131415;
    }
</style>
