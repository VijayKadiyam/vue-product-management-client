<template>
   <v-container fluid fill-height>
    <v-layout>
      <v-flex xs12>

        <v-card class="elevation-12"> 

          <v-card-title primary-title>
            <h3 class="headline mb-0">Users
              <v-btn small color="primary"
                to="/users/create"
              >Add New</v-btn>
            </h3>
            
            <v-spacer></v-spacer>

            <v-text-field
              append-icon="search"
              label="Search"
              single-line
              hide-details
              v-model="search"
            ></v-text-field>
          </v-card-title>

          <v-data-table
            :headers="headers"
            :items="items"
            :search="search"
          >
            <template slot="items" slot-scope="props"> 
              <td class="text-xs-left">{{ props.item.name }}</td> 
              <td class="text-xs-left">{{ props.item.email }}</td> 
              <td class="text-xs-left">{{ props.item.roles }}</td> 

              <td class="justify-center layout px-0">
                <v-btn icon class="mx-0"
                  :to="`/users/${props.item.id}/edit`"
                >
                  <v-icon color="teal">edit</v-icon>
                </v-btn>
                <!-- <v-btn icon class="mx-0" @click="">
                  <v-icon color="pink">delete</v-icon>
                </v-btn> -->
              </td>
            </template>
            <v-alert slot="no-results" :value="true" color="error" icon="warning">
              Your search for "{{ search }}" found no results.
            </v-alert>
          </v-data-table>

        </v-card>

      </v-flex>
    </v-layout>
  </v-container>
</template>

<script type="text/javascript">

  import Form from 'helpers/Form.js'
  
  export default {

    data: () => ({
      form: new Form,
      search: '',
      headers: [
        { text: 'Name', sortable:false, value: 'name' }, 
        { text: 'Email', sortable:false, value: 'name' }, 
        { text: 'Roles', sortable:false, value: 'roles' }, 
      ],
      items: []
    }),

    mounted() {
      this.form.get('/api/users')
        .then(data => { 
          data.data.forEach(user => {
            this.items.push({
              id: user.id,
              name: user.name,
              email: user.email,
              roles: `
                ${user.roles.map(role => role.role)}
              `
            })
          })
        })
        .catch(errors => {

        })
    }

  }
</script>