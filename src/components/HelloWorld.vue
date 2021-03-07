<template>
  <div>
    <b-row>
      <b-col></b-col>
      <b-col class="mt-4 pt-4">
        <h1>COMP 4350 Assignment</h1>
        <b-form-input v-model="form.tag" type="text" placeholder="Enter tag" required></b-form-input>
        <b-button
          type="submit"
          variant="primary"
          class="mt-4"
          @click="queryMostRecent();queryHighestRated()"
        >Submit</b-button>
      </b-col>
      <b-col></b-col>
    </b-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        tag: ""
      }
    };
  },
  methods: {
    queryMostRecent() {
      var now = new Date();
      var oneWeekAgo = new Date();
      oneWeekAgo.setDate(now.getDate() - 7);
      now = Math.floor(now.getTime() / 1000);
      oneWeekAgo = Math.floor(oneWeekAgo.getTime() / 1000);
      var tag = encodeURIComponent(this.form.tag);
      this.$axios
        .get(
          "https://api.stackexchange.com/2.2/questions?page=1&pagesize=10&fromdate=" +
            oneWeekAgo +
            "&todate=" +
            now +
            "&order=desc&sort=activity&tagged=" +
            tag +
            "&site=stackoverflow"
        )
        .then(function(response) {
          // handle success
          console.log(response.data);
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        });
    },
    queryHighestRated() {
      var now = new Date();
      var oneWeekAgo = new Date();
      oneWeekAgo.setDate(now.getDate() - 7);
      now = Math.floor(now.getTime() / 1000);
      oneWeekAgo = Math.floor(oneWeekAgo.getTime() / 1000);
      var tag = encodeURIComponent(this.form.tag);
      this.$axios
        .get(
          "https://api.stackexchange.com/2.2/questions?page=1&pagesize=10&fromdate=" +
            oneWeekAgo +
            "&todate=" +
            now +
            "&order=desc&sort=votes&tagged=" +
            tag +
            "&site=stackoverflow"
        )
        .then(function(response) {
          // handle success
          console.log(response.data);
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        });
    }
  }
};
</script>