<template>
  <div>
    <b-row>
      <b-col></b-col>
      <b-col class="mt-4 pt-4">
        <h1>COMP 4350 Assignment</h1>
        <b-form-input v-model="form.tag" type="text" placeholder="Enter tag" required></b-form-input>
        <b-button type="submit" variant="primary" class="mt-4" @click="sortQuestions()">Submit</b-button>
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
      },
      combinedQuestionsArray: []
    };
  },
  methods: {
    // queryResults() {
    //   var now = new Date();
    //   var oneWeekAgo = new Date();
    //   oneWeekAgo.setDate(now.getDate() - 7);
    //   now = Math.floor(now.getTime() / 1000);
    //   oneWeekAgo = Math.floor(oneWeekAgo.getTime() / 1000);
    //   var tag = encodeURIComponent(this.form.tag);
    //   var mostRecent =
    //     "https://api.stackexchange.com/2.2/questions?page=1&pagesize=10&fromdate=" +
    //     oneWeekAgo +
    //     "&todate=" +
    //     now +
    //     "&order=desc&sort=activity&tagged=" +
    //     tag +
    //     "&site=stackoverflow";
    //   var highestRated =
    //     "https://api.stackexchange.com/2.2/questions?page=1&pagesize=10&fromdate=" +
    //     oneWeekAgo +
    //     "&todate=" +
    //     now +
    //     "&order=desc&sort=votes&tagged=" +
    //     tag +
    //     "&site=stackoverflow";

    //     const recentRequest = this.$axios.get(mostRecent)
    //     const ratedRequest = this.$axios.get(highestRated)

    //     this.$axios.all([recentRequest,ratedRequest]).then(this.$axios.spread((recentResponse, ratedResponse) => {
    //       console.log(recentResponse.data.items)
    //       console.log(ratedResponse.data.items)
    //      var ratedArray = ratedResponse.data.items
    //      var recentArray = recentResponse.data.items
    //         this.combinedQuestionsArray = ratedArray.concat(recentArray)
    //     }))
    //     this.sortQuestions()
    // },
    sortQuestions() {
      var arr1 = [
        { creation_date: 1615102995 },
        { creation_date: 1615102927 },
        { creation_date: 1615095879 },
        { creation_date: 1615102785 },
        { creation_date: 1615063388 },
        { creation_date: 1615099540 },
        { creation_date: 1615101200 },
        { creation_date: 1615102474 },
        { creation_date: 1615101892 },
        { creation_date: 1615101775 }
      ];
      var arr2 = [
        { creation_date: 1614636045 },
        { creation_date: 1614836330 },
        { creation_date: 1614638990 },
        { creation_date: 1614819117 },
        { creation_date: 1614789789 },
        { creation_date: 1614575156 },
        { creation_date: 1614849107 },
        { creation_date: 1614673723 },
        { creation_date: 1614701299 },
        { creation_date: 1614950844 }
      ];

      this.combinedQuestionsArray = arr1.concat(arr2);
      console.log(this.combinedQuestionsArray);
      for (var x = 0; x < this.combinedQuestionsArray.length; x++) {
        console.log(this.combinedQuestionsArray[x].creation_date);
      }
      this.combinedQuestionsArray.sort(function(a,b){
        return a.creation_date - b.creation_date
      })
      console.log("after sorted");
      for (var k = 0; k < this.combinedQuestionsArray.length; k++) {
        console.log(this.combinedQuestionsArray[k].creation_date);
      }
    },
    sortResults() {
      // use selection sort to sort the combined array by time
      var length = this.combinedQuestionsArray.length;
      for (var i = 0; i < length; i++) {
        let minElement = i;
        for (var j = i + 1; j < length; j++) {
          if (
            this.combinedQuestionsArray[minElement].creation_date >
            this.combinedQuestionsArray[j].creation_date
          ) {
            minElement = j;
          }
        }
        if (minElement != i) {
          var tempElement = this.combinedQuestionsArray[i];
          this.combinedQuestionsArray[i] = this.combinedQuestionsArray[
            minElement
          ];
          this.combinedQuestionsArray[minElement] = tempElement;
        }
      }
    }
  }
};
</script>