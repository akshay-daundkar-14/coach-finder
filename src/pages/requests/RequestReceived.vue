<template>
  <div>
    <base-dialog
      :show="!!error"
      title="An error occurred!"
      @close="handleError"
    >
      <p>{{ error }}</p>
    </base-dialog>
    <section>
      <base-card>
        <header>
          <h3>Requests Received</h3>
        </header>
        <div v-if="isLoading">
          <base-spinner></base-spinner>
        </div>
        <ul v-else-if="hasRequests">
          <request-item
            v-for="request in receivedRequests"
            :key="request.id"
            :email="request.userEmail"
            :message="request.message"
          >
          </request-item>
        </ul>
        <h3 v-else>You haven't received any request yet!</h3>
      </base-card>
    </section>
  </div>
</template>


<script>
import RequestItem from '../../components/requests/RequestItem.vue';
import BaseCard from '../../components/ui/BaseCard.vue';

export default {
  data() {
    return {
      isLoading: false,
      error: null,
    };
  },
  components: {
    RequestItem,
    BaseCard,
  },
  computed: {
    receivedRequests() {
      return this.$store.getters['requests/requests'];
    },
    hasRequests() {
      return this.$store.getters['requests/hasRequests'];
    },
  },
  created() {
    this.loadRequests();
  },
  methods: {
    async loadRequests() {
      try {
        this.isLoading = true;
        await this.$store.dispatch('requests/fetchRequests');
        this.isLoading = false;
      } catch (error) {
        this.error = error || 'Something Went wrong..!';
      }
    },
    handleError() {
      this.error = null;
    },
  },
};
</script>

<style scoped>
header {
  text-align: center;
}

ul {
  list-style: none;
  margin: 2rem auto;
  padding: 0;
  max-width: 30rem;
}

h3 {
  text-align: center;
}
</style>