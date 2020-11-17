<template>
  <div>
      <div v-if="$system_variables.status_task_loaded==1">
        <a-card  v-if="$system_variables.user.id == 0">
          <button class="ant-btn ant-btn-primary ant-btn-md" @click="fbLogin">
            <img src="/images/fb.png" width="20" /> Sign In with Facebook
          </button>
        </a-card>
        <a-card  v-else>
          Please see menu for other tasks
        </a-card>
        
      </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  props: {
    msg: String
  },
  mounted:function()
  {
      this.$system_variables.status_task_loaded=1;  
      this.$system_variables.status_data_loaded=1;   
           
  },
  data() {
    return {
      value: '',
      
    }
  },
  methods: {    
    fbLogin: function()
    {
      var $root=this;
      FB.getLoginStatus(function(responseAlredy) {   // See the onlogin handler
        if (responseAlredy.status === 'connected') {
          $root.loginUser(responseAlredy.authResponse)
        } 
        else {
          FB.login(function(responseLogin) {            
            if (responseLogin.status === 'connected') {
              $root.loginUser(responseLogin.authResponse);
            }
            else {
              console.log("Login Denied");
            }
          });
        }
      });
    },
    loginUser:function(authResponse)
    {
      var $root=this;
      console.log(authResponse);
      //authResponse.accessToken      
      //authResponse.signedRequest      
      //authResponse.userID     =10157970693107987 
      FB.api(
          authResponse.userID,
          function (responseName) {            
            if (responseName && !responseName.error) {
              $root.$system_variables.user['id'] = authResponse.userID;  
              $root.$system_variables.user['name'] = responseName.name;  
              FB.api(
                '/'+authResponse.userID+'/picture?redirect=false',                
                function(responsePicture) 
                {
                  if(responsePicture.data)
                  {
                    
                    $root.$system_variables.user.profile_picture = responsePicture.data.url;                                        
                  }                  
                }
              );

            }
          }
      );
      
    }
  },
}
</script>
