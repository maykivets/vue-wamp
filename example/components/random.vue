<template>
    <div class="component-random">
        <div class="row">
            <span class="col-xs-8">Length</span>
            <div class="col-xs-4 text">
                <input class="form-control" v-model.number="input.length" type="number" min="1" max="1024"/>
            </div>
        </div>
        <div class="row">
            <span class="col-xs-8">Count</span>
            <div class="col-xs-4">
                <input class="form-control" v-model.number="input.count" type="number" min="1" max="1024"/>
            </div>
        </div>
        <div class="row">
            <span class="col-xs-8">Type</span>
            <div class="col-xs-4">
                <select class="form-control" v-model="input.type">
                    <option v-for="(name, type) in types" :value="type">{{ name }}</option>
                </select>
            </div>
        </div>
        <div class="row">
            <div class="col-xs-8">
                <div class="pull-left result-value" v-for="v in values">
                    <span class="label label-primary">{{ v }}</span>
                </div>
                <div class="clearfix"></div>
            </div>
            <div class="col-xs-4">
                <button @click="generate" class="form-control btn btn-default" :disabled="working">
                    <span v-if="working"><i class="glyphicon glyphicon-repeat normal-right-spinner"></i></span>
                    <span v-else>Generate</span>
                </button>
            </div>
        </div>
    </div>
</template>

<style>
    .component-random {
    }

    .component-random button {
        text-align: center;
    }

    .component-random .result-value {
        font-family: "Lucida Console", Monaco, monospace;
        padding: 1px 2px;
    }

    .component-random .result-value > .label {
        padding: 4px 6px;
    }
</style>

<script>

  import {RPC_RANDOM} from '../common.js'

  export default {
    data() {
      return {
        types: {
          'aA': 'Alpha',
          'a': 'Alpha Lowercase',
          'A': 'Alpha Uppercase',
          'aA#': 'Alpha Numeric',
          '#': 'Numeric',
          'aA#!': 'Special Characters',
        },
        working: false,
        values: [],
        input: {
          length: 12,
          count: 3,
          type: 'aA#',
        }
      }
    },
    methods: {
      generate() {
        this.working = true;
        this.$wamp.call(RPC_RANDOM, [], this.input).then(
          r => {
            this.working = false;
            this.values = r;
          },
          e => {
            this.working = false;
            this.values = [];
            console.error(e);
          }
        );
      }
    },
    wamp: {
      register: {
      }
    }
  }
</script>
