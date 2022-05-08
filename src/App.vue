<template>
  <header>
        <nav class="navbar">
        <RouterLink to="/" class="link">Home</RouterLink>
      </nav>

      <div class="content-area">
      <div class="content">

        <h1>QuranKu</h1>
        <p>membaca dan mendengarkan Al-quran dengan mudah</p>
    </div>
    </div>

      <nav class="navbar2">
        <RouterLink to="/QuranKu" class="link">Klik Disini</RouterLink>
      </nav>
  </header>

  <RouterView />

  
</template>

<style>
#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  min-height: 100vh;
  background-color: #c2c2b4 ;
  font-weight: normal;
}

.navbar{
  display: flex;
  flex-direction: row;
  justify-content: center;
}

.content h1{
  font-size: 80px;
  color: #163317;
  justify-content: center;
  padding: auto;
  margin-top: 50px;
}

.content p{
  font: size 30px;
  color: #163317;
}

.navbar2{
  display: flex;
  flex-direction: row;
  justify-content: center;
  border: none;
  outline: none;
  padding: 15px;
  margin-top: 20px;
  border-radius: 4px;
  
  font-size:18px ;
  cursor: pointer;
  background: #163317;
}

.link{
  color: white;
  font-size: 20px;
  padding: 10px;
  text-decoration: none;
}
</style>