<template>
  <b-card title="Status of Room 3">
    <b-card-text>
      <b-list-group>
        <b-list-group-item
          v-for="item in this.items"
          :key="item.text"
          class="d-flex justify-content-between align-items-center"
        >
          <b-icon icon="x-square-fill" scale="1.5" variant="danger"></b-icon>
          {{ item.text }}
        </b-list-group-item>
        <b-list-group-item class="d-flex justify-content-between align-items-center">
          <b-icon icon="exclamation-triangle-fill" scale="1.5" variant="warning"></b-icon>
          Dapibus ac facilisis in
        </b-list-group-item>
        <b-list-group-item class="d-flex justify-content-between align-items-center">
          <b-icon icon="info-circle" scale="1.5" variant="info"></b-icon>
          Morbi leo risus
        </b-list-group-item>
        <b-list-group-item class="d-flex justify-content-between align-items-center">
          <b-icon icon="check-square" scale="1.5" variant="success"></b-icon>
          Incididunt veniam velit
        </b-list-group-item>
      </b-list-group>
    </b-card-text>
  </b-card>
</template>
<script>
export default {
  props: {
    items: {
      type: Array,
      default: () => []
    }
  },
  mounted() {}
}
</script>
