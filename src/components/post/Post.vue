<template>
  <div>
      <div v-if="$system_variables.status_task_loaded==1">        
        <a-card>
          <a-row>
            <a-col :sm="12" :lg="10">
              <a-input v-bind="post_id"  placeholder="Post Id" />
            </a-col>
            <a-col :sm="{ span: 6, offset: 1 }">
              <a-button type="primary" @click="getPost">get post info</a-button>
            </a-col>
            
          </a-row>
          
        </a-card>
        
      </div>
  </div>
</template>

<script>
export default {
  name: 'Post',
  props: {
    msg: String
  },
  mounted:function()
  {
    if(!(this.$system_variables.user.id >0))
    {
      this.$router.push("/");
    }
      this.$system_variables.status_task_loaded=1;  
      this.$system_variables.status_data_loaded=1;   
           
  },
  data() {
    return {
      post_id: '',
      
    }
  },
  methods: {    
    getPost: function()
    {
      var $root=this;
      FB.getLoginStatus(function(responseAlredy) {   // See the onlogin handler
        if (responseAlredy.status === 'connected') {
          FB.api(
            '/'+$root.$system_variables.user.id+'/photos',
            'GET',
            {},
            function(response) {
                console.log(response);
            }
          );
          console.log(this.post_id);          
        } 
        else {
          $root.$router.push("/");
        }
      });
    },
    
  },
}
</script>
