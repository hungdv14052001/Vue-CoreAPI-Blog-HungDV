<template>
    <div id="Header">
        <div class="headerLogo">
          <img class="imgLogo" src="../assets/images/devfast-logo.png" alt="Logo">
        </div>
        <div class="HeaderContent">
          Blogmanagement
        </div>
    </div>
</template>
<script>
export default {
  name: 'AppHeader'
}
</script>
<style>
  #Header{
    height: 60px;
    width: 100%;
    background-color: #201E1E;
    display: flex;
    justify-content: center;
  }
  .headerLogo{
      width: 17%;
      height: 100%;
      display: flex;
      justify-content: center;
  }
  .imgLogo{
      height: 100%;
  }
  .HeaderContent{
      width: 83%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
      font-size: 23px;
      font-family: Arial;
  }
</style>