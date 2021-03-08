<template>
  <div>
    <b-row>
      <b-col></b-col>
      <b-col class="mt-4 pt-4">
        <h1>COMP 4350 Assignment</h1>
        <b-form-input v-model="form.tag" type="text" placeholder="Enter tag" required></b-form-input>
        <b-button type="submit" variant="primary" class="mt-4" @click="submit()">Submit</b-button>
        <div class="mt-4" v-for="item in listObject" :key="item.question_id">
          <b-button
            v-b-toggle="'collapse-' + item.question_id"
            class="m-1"
          >{{item.questionBody.title}}</b-button>
          <b-collapse :id="'collapse-' + item.question_id">
            <b-card>
              <h2>Question {{item.question_id}}</h2>
              <div v-html="item.questionBody.body"></div>
            </b-card>
            <div v-for="comment in item.questionCommentList" :key="comment.comment_id">
              <b-card>
                <h2>Comment {{comment.comment_id}}</h2>
                <div v-html="comment.body"></div>
              </b-card>
            </div>
            <div v-for="answer in item.answerList" :key="answer.answer_id">
              <b-card>
                <h2>Answer {{answer.answer_id}}</h2>
                <div v-html="answer.body"></div>
              </b-card>
              <div v-for="answerComment in answer.answerComments" :key="answerComment.comment_id">
                <b-card>
                  <h2>Comment {{answerComment.comment_id}}</h2>
                  <div v-html="answerComment.body"></div>
                </b-card>
              </div>
            </div>
          </b-collapse>
        </div>
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
      listObject: [],
      searchComplete: false
    };
  },
  methods: {
    async submit() {
      var questionsArray = await this.queryResults();
      console.log(questionsArray);

      var questionIDString = await this.getQuestionIDString(questionsArray);
      console.log(questionIDString);

      var answersArray = await this.getAnswers(questionIDString);
      console.log(answersArray);

      var answerIDsString = await this.getAnswersIDString(answersArray);
      console.log(answerIDsString);

      var commentsForQuestionsArray = await this.getCommentsForQuestion(
        questionIDString
      );
      console.log(commentsForQuestionsArray);

      var commentsForAnswersArray = await this.getCommentsForAnswers(
        answerIDsString
      );
      console.log(commentsForAnswersArray);

      var testObj = this.createPostArray(
        questionsArray,
        answersArray,
        commentsForQuestionsArray,
        commentsForAnswersArray
      );
      console.log(testObj);
      this.listObject = testObj;
      this.searchComplete = true;
    },
    async queryResults() {
      var now = new Date();
      var oneWeekAgo = new Date();
      oneWeekAgo.setDate(now.getDate() - 7);
      now = Math.floor(now.getTime() / 1000);
      oneWeekAgo = Math.floor(oneWeekAgo.getTime() / 1000);
      var tag = encodeURIComponent(this.form.tag);
      var ratedArray = [];
      var recentArray = [];
      await this.$axios
        .get(
          "https://api.stackexchange.com/2.2/questions?page=1&pagesize=1&fromdate=" +
            oneWeekAgo +
            "&todate=" +
            now +
            "&order=desc&sort=activity&tagged=" +
            tag +
            "&site=stackoverflow&filter=withbody"
        )
        .then(function(response) {
          // handle success
          recentArray = response.data.items;
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        });

      await this.$axios
        .get(
          "https://api.stackexchange.com/2.2/questions?page=1&pagesize=1&fromdate=" +
            oneWeekAgo +
            "&todate=" +
            now +
            "&order=desc&sort=votes&tagged=" +
            tag +
            "&site=stackoverflow&filter=withbody"
        )
        .then(function(response) {
          // handle success
          ratedArray = response.data.items;
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        });

      this.combinedQuestionsArray = ratedArray.concat(recentArray);
      //Sort the questions
      //use Array.prototype.sort https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
      this.combinedQuestionsArray.sort(function(a, b) {
        return a.creation_date - b.creation_date;
      });
      return this.combinedQuestionsArray;
    },
    async getQuestionIDString(questionArray) {
      //Get the ID's of each question
      var questionIDsString = "";
      for (var x = 0; x < questionArray.length; x++) {
        questionIDsString += ";" + questionArray[x].question_id;
      }
      questionIDsString = questionIDsString.substring(1); //remove first semicolon
      return questionIDsString;
    },
    async getAnswersIDString(answerArray) {
      //Get the ID's of each question
      var answerIDsString = "";
      for (var x = 0; x < answerArray.length; x++) {
        answerIDsString += ";" + answerArray[x].answer_id;
      }
      answerIDsString = answerIDsString.substring(1); //remove first semicolon
      return answerIDsString;
    },

    async getAnswers(questionIDsString) {
      var answerArray = [];
      await this.$axios
        .get(
          "https://api.stackexchange.com/2.2/questions/" +
            questionIDsString +
            "/answers?page=1&pagesize=100&order=desc&sort=activity&site=stackoverflow&filter=withbody"
        )
        .then(function(response) {
          // handle success
          answerArray = response.data.items;
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        });
      return answerArray;
    },
    async getCommentsForQuestion(questionIDsString) {
      var questionComments = [];
      await this.$axios
        .get(
          "https://api.stackexchange.com/2.2/posts/" +
            questionIDsString +
            "/comments?order=desc&sort=creation&site=stackoverflow&filter=withbody"
        )
        .then(function(response) {
          // handle success
          questionComments = response.data.items;
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        });
      return questionComments;
    },
    async getCommentsForAnswers(answerIDsString) {
      var answerComments = [];
      await this.$axios
        .get(
          "https://api.stackexchange.com/2.2/posts/" +
            answerIDsString +
            "/comments?order=desc&sort=creation&site=stackoverflow&filter=withbody"
        )
        .then(function(response) {
          // handle success
          answerComments = response.data.items;
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        });
      return answerComments;
    },
    createPostArray(
      questionsArray,
      answersArray,
      commentsForQuestionsArray,
      commentsForAnswersArray
    ) {
      //Map the comments,questions and answers all the one post id
      var postArray = [];
      var postObject = {};
      for (var i = 0; i < questionsArray.length; i++) {
        var questionID = questionsArray[i].question_id;
        var questionCommentFilter = commentsForQuestionsArray.filter(
          comment => comment.post_id === questionID
        );
        var answerFilter = answersArray.filter(
          answer => answer.question_id === questionID
        );

        for (var k = 0; k < answerFilter.length; k++) {
          var answerID = answerFilter[k].answer_id;

          var answerCommentFilter = commentsForAnswersArray.filter(
            comment => comment.post_id === answerID
          );
          Object.assign(answerFilter[k], {
            answerComments: answerCommentFilter
          });
        }
        postObject = {
          question_id: questionID,
          answerList: answerFilter,
          questionCommentList: questionCommentFilter,
          questionBody: questionsArray[i]
        };
        postArray.push(postObject);
      }
      return postArray;
    }
  }
};
</script>