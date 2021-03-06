<template>
  <div class="omegaup-admin-user panel-primary panel">
    <div class="panel-heading">
      <h2 class="panel-title">{{ T.omegaupTitleAdminUsers }} &mdash; {{ username }}</h2>
    </div>
    <div class="panel-body">
      <form class="form bottom-margin" v-on:submit.prevent="onChangePassword">
        <div class="row">
          <div class="col-md-6">
            <div class="input-group">
              <input name="password" v-model="password" type="text" class="form-control" v-bind:placeholder="T.passwordResetPassword" />
              <span class="input-group-btn">
                <button class="btn btn-default" type="button" v-on:click.prevent="onGeneratePassword" v-bind:aria-label="T.passwordGenerateRandom" v-bind:title="T.passwordGenerateRandom">
                  <span class="glyphicon glyphicon-random" aria-hidden="true"></span>
                </button>
                <button class="btn btn-default" type="button" v-on:click.prevent="onChangePassword">{{ T.userEditChangePassword }}</button>
              </span>
            </div>
          </div>
          <div class="col-md-6">
            <button type="button" v-bind:disabled="verified" class="btn btn-default btn-block" v-on:click.prevent="onVerifyUser">
              <template v-if="verified">
                <span class="glyphicon glyphicon-ok" aria-hidden="true"></span> {{ T.userVerified }}
              </template>
              <template v-else>
                {{ T.userVerify }}
              </template>
            </button>
          </div>
        </div>
      </form>
      <h4>{{ T.userEmails }}</h4>
      <ul class="list-group">
        <li class="list-group-item" v-for="email in emails">{{ email }}</li>
      </ul>
      <h4>{{ T.userRoles }}</h4>
      <table class="table">
        <tbody>
          <tr v-for="role in roleNames">
            <td><input type="checkbox" v-bind:disabled="role == 'Admin'" v-bind:checked="hasRole(role)" v-on:change.prevent="onChangeRole($event, role)" /></td>
            <td>{{ role }}</td>
          </tr>
        </tbody>
      </table>
      <h4>{{ T.wordsExperiments }}</h4>
      <table class="table">
        <tbody>
          <tr v-for="experiment in systemExperiments">
            <td><input type="checkbox" v-bind:disabled="experiment.config" v-bind:checked="experiment.config || hasExperiment(experiment)" v-on:change.prevent="onChangeExperiment($event, experiment)" /></td>
            <td>{{ experiment.name }}</td>
            <td>{{ experiment.hash }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import DatePicker from '../DatePicker.vue';
import {T} from '../../omegaup.js';

export default {
  props: {
    emails: Array,
    experiments: Array,
    systemExperiments: Array,
    roleNames: Array,
    roles: Array,
    username: String,
    verified: Boolean,
  },
  data: function() {
    return {
      T: T,
      password: '',
    };
  },
  methods: {
    hasExperiment: function(name) {
      return this.experiments.indexOf(name) !== -1;
    },
    hasRole: function(name) {
      return this.roles.indexOf(name) !== -1;
    },
    onChangeExperiment: function(ev, experiment) {
      this.$emit('change-experiment', experiment, ev.target.checked);
    },
    onChangePassword: function() {
      this.$emit('change-password', this.password);
    },
    onChangeRole: function(ev, role) {
      this.$emit('change-role', role, ev.target.checked);
    },
    onVerifyUser: function() {
      this.$emit('verify-user');
    },
    onGeneratePassword: function() {
      let chars = 'abcdefghjkmnpqrstuvwxyzABCDEFGHJKLMNPQRSTUVWXYZ23456789';
      let length = 8;
      var newPassword = '';
      for (var i = 0; i < length; i++) {
        newPassword += chars[Math.floor(Math.random() * chars.length)];
      }
      this.password = newPassword;
    },
  },
};
</script>
