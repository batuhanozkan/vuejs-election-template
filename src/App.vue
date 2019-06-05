<template>
  <div>
    <section class="is-fullheight is-turquoise" style="min-height:100vh;">
      <!-- Navbar -->
      <div class="container">
        <div class="columns is-mobile is-multiline">
          <div class="column is-2">
            <SidebarMenu width="320px"/>
          </div>
          <div class="column is-10">
            <div class="column is-10" style="position: relative;left: 1%;">
              <b-radio
                v-model="radio"
                v-for="(cand, index) in data"
                :key="index"
                v-bind:native-value="cand.id"
                size="is-large"
              >{{cand.name}}</b-radio>

              <br>
              <br>
              <b-button @click="clickMe">Vote</b-button>
            </div>

            <div class="column is-12" style="position: relative;left: 1%;">
              <h1 class="title">{{electionName}}</h1>
            </div>

            <div class="column">
              <div class="column is-16">
                <b-table :data=" data">
                  <template slot-scope="props">
                    <b-table-column field="name" label="Candidate">
                      {{ props.row.name }}
                      <br>
                      <br>
                      <p style="font-size:70%">Votes:{{props.row.vote}}</p>

                      <div
                        :class="
                            [
                                'tag',
                                {'is-danger': 10 /3 <= 0.45},
                                {'is-success': 10 / 2 > 0.45}
                            ]"
                        style="height:10px"
                        v-bind:style="{width:props.row.rate+'%'}"
                      ></div>

                      <br>
                    </b-table-column>

                    <b-table-column field="result" label="Result" centered>
                      <div style=" font-size:300%">%{{ props.row.rate}}</div>
                    </b-table-column>
                  </template>
                </b-table>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  methods: {
    clickMe() {
      this.data[this.radio].total = 0;
      this.data[this.radio].vote++;
      var i = 0;
      for (i in this.data) {
        this.data[this.radio].total =
          this.data[this.radio].total + this.data[i].vote;
      }

      for (i in this.data) {
        this.data[i].rate = Math.round(
          (this.data[i].vote / this.data[this.radio].total) * 100
        );
      }
    }
  },
  name: "app",
  data() {
    var vote = 0;
    var total = 0;
    const data = [
      {
        id: "0",
        name: "Flint",
        vote: 0,
        total: 0,
        rate: 0
      },
      {
        id: "1",
        name: "Silver",
        vote: 0,
        total: 0,
        rate: 0
      },
      {
        id: "2",
        name: "Jack",
        vote: 0,
        total: 0,
        rate: 0
      }
    ];
    return {
      electionName: "Sample Election",

      radio: "default",
      data
    };
  }
};
</script>

<style>
</style>
