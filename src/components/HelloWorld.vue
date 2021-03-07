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
      combinedQuestionsArray: [],
      combinedAnswersArray: [],
      questionIDsArray: [],
      answerIDsArray: [],
      questionIDsString: "",
      answerIDsString: ""
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
    //     "&site=stackoverflow&filter=withbody";
    //   var highestRated =
    //     "https://api.stackexchange.com/2.2/questions?page=1&pagesize=10&fromdate=" +
    //     oneWeekAgo +
    //     "&todate=" +
    //     now +
    //     "&order=desc&sort=votes&tagged=" +
    //     tag +
    //     "&site=stackoverflow&filter=withbody";

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
        { creation_date: 1615102995,questionID:66430308 },
        { creation_date: 1615102927,questionID:66430308 },
        { creation_date: 1615095879,questionID:66430308 },
        { creation_date: 1615102785,questionID:66430308 },
        { creation_date: 1615063388,questionID:66430308 },
        { creation_date: 1615099540,questionID:66430308 },
        { creation_date: 1615101200,questionID:66430308 },
        { creation_date: 1615102474,questionID:66430308 },
        { creation_date: 1615101892,questionID:66430308 },
        { creation_date: 1615101775,questionID:66430308 }
      ];
      var arr2 = [
        { creation_date: 1614636045,questionID:66430308 },
        { creation_date: 1614836330,questionID:66430308 },
        { creation_date: 1614638990,questionID:66430308 },
        { creation_date: 1614819117,questionID:66430308 },
        { creation_date: 1614789789,questionID:66430308 },
        { creation_date: 1614575156,questionID:66430308 },
        { creation_date: 1614849107,questionID:66430308 },
        { creation_date: 1614673723,questionID:66430308 },
        { creation_date: 1614701299,questionID:66430308 },
        { creation_date: 1614950844,questionID:66430308 }
      ];

      this.combinedQuestionsArray = arr1.concat(arr2);

      //Get the ID's of each question
      for (var x = 0; x < this.combinedQuestionsArray.length; x++) {
       this.questionIDsArray[x] = this.combinedQuestionsArray[x].questionID;
      }
      //Sort the questions
      //use Array.prototype.sort https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
      this.combinedQuestionsArray.sort(function(a,b){
        return a.creation_date - b.creation_date
      })

      //Stringify the question ID's with semicolon delimiter 
        for (var item of this.questionIDsArray) {
        this.questionIDsString += ";"+item
      }
      this.questionIDsString= this.questionIDsString.substring(1) //remove first semicolon
      console.log(this.questionIDsString)
    },
  
  
    getAnswers() {


      // this.$axios
      //   .get(
      //     "/2.2/questions/"+questionID+"/answers?page=1&pagesize=100&order=desc&sort=activity&site=stackoverflow"
      //   )
      //   .then(function(response) {
      //     // handle success
      //      this.combinedAnswersArray = ratedResponse.data.items

      //   })
      //   .catch(function(error) {
      //     // handle error
      //     console.log(error);
      //   });

      //Stringify the answer ID's with semicolon delimiter 
      for (var x = 0; x < this.combinedAnswersArray.length; x++) {
       this.answerIDsArray[x] = this.combinedAnswersArray[x].questionID;
      }
      
    },
    getCommentsForQuestion() {

      // this.$axios
      //   .get(
      //     "https://api.stackexchange.com/2.2/posts/"+this.questionIDsString+"/comments?order=desc&sort=creation&site=stackoverflow&filter=withbody"
      //   )
      //   .then(function(response) {
      //     // handle success
      //      this.combinedAnswersArray = ratedResponse.data.items

      //   })
      //   .catch(function(error) {
      //     // handle error
      //     console.log(error);
      //   });
      
    },
    getCommentsForAnswers() {

      // this.$axios
      //   .get(
      //     ""https://api.stackexchange.com/2.2/posts/"+this.answerIDsArray+"/comments?order=desc&sort=creation&site=stackoverflow&filter=withbody""
      //   )
      //   .then(function(response) {
      //     // handle success
      //      this.combinedAnswersArray = ratedResponse.data.items

      //   })
      //   .catch(function(error) {
      //     // handle error
      //     console.log(error);
      //   });
      
    },
    createObjectMap(){
      //Map the comments,questions and answers all the one post id

      
    }
  
}

};
</script>