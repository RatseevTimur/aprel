<template>
    <div class="card">
        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRyyY7WzBoV_m3HnhW7hk2TXoxzGpC05VuWXT6PAvCW&s" alt="John" style="width:100%">
        <h1>User</h1>
        <p class="title">Частное лицо</p>
        <p>Краснодар</p>
        <a href="#"><i class="fa fa-dribbble"></i></a>
        <a href="#"><i class="fa fa-twitter"></i></a>
        <a href="#"><i class="fa fa-linkedin"></i></a>
        <a href="#"><i class="fa fa-facebook"></i></a>
        <button class="button" @click="contact()">Contact</button>

        <modal name="contact">
            <div class="contact">
                <p>Рацеев Тимур</p>
                <p>8-928-757-29-38</p>
                <a href="https://github.com/RatseevTimur/aprel">This Project</a>
            </div>
        </modal>
    </div>
</template>

<script>
export default {
    methods: {
        hide () {
            this.$modal.hide('contact');
        },
        contact(product){
            this.$modal.show('contact');
        },
        mount () {
            this.show()
        },
    }
}
</script>

<style>
.contact{
    margin-top: 100px;
    position: relative;
    height: 100%;
    width: 100%;
    text-align: center;
    vertical-align: middle;
    align-content: center;
    justify-content: center;
}
.card {
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
    max-width: 300px;
    margin: auto;
    text-align: center;
}

.title {
    color: grey;
    font-size: 18px;
}

.button {
    margin: 0;
    border: none;
    outline: 0;
    display: inline-block;
    padding: 8px;
    color: white;
    background-color: #000;
    text-align: center;
    cursor: pointer;
    width: 100%;
    font-size: 18px;
}

a {
    text-decoration: none;
    font-size: 22px;
    color: black;
}

button:hover, a:hover {
    opacity: 0.7;
}
</style>