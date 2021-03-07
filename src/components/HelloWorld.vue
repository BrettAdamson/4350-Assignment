<template>
  <div>
    <b-row>
      <b-col></b-col>
      <b-col class="mt-4 pt-4">
        <h1>COMP 4350 Assignment</h1>
        <b-form-input v-model="form.tag" type="text" placeholder="Enter tag" required></b-form-input>
        <b-button type="submit" variant="primary" class="mt-4" @click="queryMostRecent()">Submit</b-button>
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
     // var numResults = 10;
     var tag = encodeURIComponent(this.form.tag)
      this.$axios
        .get("https://api.stackexchange.com/2.2/questions?page=1&pagesize=10&fromdate=1614470400&todate=1614988800&order=desc&sort=activity&tagged="+tag+"&site=stackoverflow")
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