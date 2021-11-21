.
<template>
  <div class="about-wrapper">
        <div class="about-left">
            <div class="about-left-content">
                <div>
                    <div class="shadow">
                        <div class="about-img">
                            <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTGf7VYZTDBo1FycrzMbPy5y-a-1NevPraVcd-aU95IiTJcT4CXxliwGTqsHcuXH-MxEWU&usqp=CAU" alt="about image">
                        </div>
                    </div>

                    <h2>Your TODO List</h2>
                    <h3>Your professional website</h3>
                </div>

                <ul class="icons">
                    <li><i class="fab fa-facebook-f"></i></li>
                    <li><i class="fab fa-twitter"></i></li>
                    <li><i class="fab fa-linkedin"></i></li>
                    <li><i class="fab fa-instagram"></i></li>
                </ul>
            </div>

        </div>

        <div class="about-right">
            <h1>What are you doing<span>?</span></h1>
            <h2>I make everything easy for you</h2>
            <div class="about-btns">
                  <router-link to="/signin" tag="button" class="btn btn-pink">sign in</router-link>
                  <router-link to="/signup" tag="button" class="btn btn-white">sign up</router-link>
            </div>

            <div class="about-para">
                <p>Create clear, multi-functional to-do lists to easily manage your ideas and work from anywhere so you never forget anything again.</p>
                <p>Save any list as a template to reuse and customize as many times as you want. You can even create recurring tasks with saved checklists for error-free processes.</p>
            </div>
            <div class="credit">Made with <span style="color:tomato">❤</span> Developer</div>
        </div>
    </div>
</template>

<script>
export default {};
</script>

<style>
</style>
