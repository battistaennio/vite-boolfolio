<script>

export default{
  name: 'AppHeader',
}
</script>

<template>

  <header>

    <h2>Portfolio</h2>

      <nav>

        <ul>
          <li>
            <router-link :to="{name: 'home'}">Home</router-link>
            <router-link :to="{name: 'projects'}">Progetti</router-link>
          </li>
        </ul>

      </nav>
  </header>

</template>

<style lang="scss">

header{
  background-color: black;
  color: white;
  height: 50px;
  padding: 0 10px;

  display: flex;
  justify-content: space-between;
  align-items: center;

  li{
    list-style: none;
    display: inline-block;

    a{
      text-decoration: none;
      color: white;
      margin: 0 5px;

      &:hover{
        text-decoration: underline;
        cursor: pointer;
        color: blueviolet;
      }
    }
  }
}

</style>