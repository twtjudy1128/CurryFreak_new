<template>
  <div class="column">
    <div class="card">
      <header class="headercolor card-header">
        <p class="card-header-title has-text-grey">
          {{ title }}
        </p>
      </header>
      <div class="card-content">        
        <figure class="image is-square">
          <img v-bind:src="url"/>
        </figure>
      </div>
      <footer class="card-footer">
        <div class="card-footer-item">
          <span>
            <slot />
          </span>
        </div>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    },
    url: {
      type: String,
      required: true
    }
  }
}
</script>

<style>
.headercolor{
  background-color:gold;
}
</style>
