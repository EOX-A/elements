<template>
  <loading-indicator v-if="!component" />
  <component
    v-else
    :is="component"
    v-bind="{
      ...$attrs,
      ...$props,
    }"
    v-on="$listeners"
  />
</template>

<script>
  export default {
    name: 'VueFile',
    inheritAttrs: false,
    props: {
      file: {
        type: String,
        required: true,
      },
    },
    data: () => ({ component: undefined }),
    created () {
      this.load()
    },
    methods: {
      async load () {
        let component = {}
        try {
          component = await import(
            /* webpackChunkName: "examples" */
            /* webpackMode: "lazy-once" */
            // `../../examples/${this.file}.vue`
            `./${this.file}`
          )
          this.$emit('loaded', component.default)
        } catch (err) {
          // component = await import('./ExampleMissing')
          this.$emit('error', err)
        }
        this.component = component.default
      },
    },
  }
</script>