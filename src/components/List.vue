<template>
  <b-card :title="title">
    <b-card-text>
      <b-list-group>
        <b-list-group-item
          v-for="item in this.items"
          :key="item.text"
          class="d-flex justify-content-between align-items-center"
        >
          <b-icon v-if="item.type === 'danger'" icon="x-square-fill" scale="1.5" variant="danger"></b-icon>
          <b-icon v-else-if="item.type === 'success'" icon="check-square" scale="1.5" variant="success"></b-icon>
          <b-icon
            v-else-if="item.type === 'warning'"
            icon="exclamation-triangle-fill"
            scale="1.5"
            variant="warning"
          ></b-icon>
          <b-icon v-else icon="info-circle" scale="1.5" variant="info"></b-icon>
          {{ item.text }}
        </b-list-group-item>
      </b-list-group>
    </b-card-text>
  </b-card>
</template>
<script>
export default {
  props: {
    title: {
      type: String,
      default: ''
    },
    items: {
      type: Array,
      default: () => []
    }
  },
  mounted() {}
}
</script>
