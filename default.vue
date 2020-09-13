<template>
<div>
    <nav
      class="color navbar header has-shadow is-Primary"
      role="navigation"
      aria-label="main navigation"
    >
      <div class="navbar-brand">
        <a
          class="navbar-item"
          href="/"
        >
          <!--<img
            src="~assets/buefy.png"
            alt="Buefy"
            height="28"
          >-->
        <p class="title is-1">Curry Freak</p>
        </a>

        <div class="navbar-burger">
          <span />
          <span />
          <span />
        </div>
      </div>
    </nav>

    <section class="main-content columns">
      <aside class="column is-2 section">
        <p class="menu-label is-hidden-touch">
        
        </p>
        <ul class="menu-list">
          <li
            v-for="(item, key) of items"
            :key="key"
          >
            <nuxt-link
              :to="item.to"
              exact-active-class="is-active"
            >
              <b-icon :icon="item.icon" /> {{ item.title }}
            </nuxt-link>
          </li>
        </ul>
      </aside>

      <div class="container column is-10">
        <nuxt />
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data () {
    return {
      items: [
        {
          title: 'Curry一覧',
          icon: 'home',
          to: { name: 'index' }
        },
        {
          title: '新しいCurryを追加',
          icon: 'plus-thick',
          to: { name: 'post' }
        }
      ]
    }
  }
}
</script>

<style>
.color {
  background-color:gold;
  }
</style>
