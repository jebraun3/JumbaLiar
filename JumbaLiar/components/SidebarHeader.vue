<template>
  <div class="header">
    <img class="icon" src="./icons/SingleShrimp.png" />
    <p>JumbaLiar</p>
  </div>
</template>

<style scoped>
.header {
  display: flex;
  background-color: var(--sidebar-dark);
  box-shadow: 4px 4px 4px rgba(0, 0, 0, 0.25);
  height: var(--header-height);
}

.icon {
  object-fit: fill;
  height: var(--header-shrimp-height);
  width: var(--header-shrimp-height);
  margin: auto;
  margin-right: var(--header-pfp-margins);
  margin-bottom: var(--header-pfp-margins);
  margin-top: var(--header-pfp-margins);
}

p {
  font-size: var(--header-font-size);
  color: white;
  margin: auto;
  text-align: left;
  margin-left: 0.5rem;
}
</style>
