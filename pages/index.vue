<template>

  <AdminTemplate>
    <div slot="body">

      h3
    </div>

  </AdminTemplate>
  </template>

<script>
import AdminTemplate from '../components/AdminTemplate.vue';

export default {
    name: 'IndexPage',
    head() {
        return {
            title: 'Index'
        };
    },
    components: { AdminTemplate }
}
</script>
