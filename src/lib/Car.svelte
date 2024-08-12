<script>
    export let car;
    export let description;
    export let price;
    export let img;
    export let credit;
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
        width: 300px;
    }

    h2 {
        color: #171d23;
        font-family: "Audiowide", sans-serif;
        font-weight: 200;
        font-size: 1.6rem;
    }

    h3 {
        color: #171d23;
        font-family: "Raleway", sans-serif;
        font-weight: 500;
    }

    p {
        color: #171d23b6;
        font-family: "Raleway", sans-serif;
        font-weight: 500;
    }
</style>
