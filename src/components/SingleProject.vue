<template>
  <div class="project" :class="{complete: project.complete}">
      <div class="actions">
          <h3 @click="showParagraph">{{ project.title }}</h3>
          <div class="icons">
              <router-link :to="{ name : 'EditProject', params: { id: project.id }}">
                  <span class="material-icons">edit</span>
              </router-link>
              <span class="material-icons" @click="deleteProject">delete</span>
              <span class="material-icons tick" @click="toggleComplete">done</span>
          </div>
      </div>
      <div class="details" v-if="showPara">
          
          <p >{{ project.details }}</p>
          
      </div>
  </div>
</template>

<script>
export default {
    props: ['project'],
    data(){
        return {
            showPara: false,
            uri: 'http://localhost:3000/projects/' + this.project.id
        }
    },

    methods: {
        showParagraph(){
            this.showPara = !this.showPara;
        },
        deleteProject(){
            
            fetch(this.uri, { method: 'DELETE', })
            .then(() => this.$emit('delete', this.project.id))
            .catch(err => console.log(err))
        },
        toggleComplete(){
            fetch(this.uri, {
                method: 'PATCH',
                headers: {'content-Type': 'application/json'},
                body: JSON.stringify({ complete: !this.project.complete })
            }).then(() => {
                this.$emit('complete', this.project.id)
            }).catch(err => console.log(err))
        }
    }
}
</script>

<style scoped>
 .project {
     margin: 20px auto;
     background-color: white;
     padding: 10px 20px;
     border-radius: 4px;
     box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
     border-left: 4px solid rgb(109, 0, 0);
 }

 h3{
     cursor: pointer;
 }

 .actions {

     display: flex;
     align-items: center;
     justify-content: space-between;
 }

 .material-icons {
     font-size: 24px;
     margin-left: 7px;
     color: #bbb;
     cursor: pointer;
 }

 .material-icons:hover {
     color: #777;
 }

 .project.complete {
     border-left: 4px solid rgb(1, 243, 130);
 }

 .project.complete .tick {
     color: rgb(1, 243, 130);
 }
</style>