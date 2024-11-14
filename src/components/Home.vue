<template>
    <v-container fluid class="pa-4">
      <v-row>
        <!-- Coluna para a lista de blocos de tarefas -->
        <v-col cols="12" md="3" class="pa-2">
          
            <v-row 
            style="margin-bottom: 20px;display: flex;flex-direction: row;justify-content: space-around;align-items: baseline;">
                <v-card-title class="mb-2">Blocos de Tarefas</v-card-title>
          <v-btn icon @click="addBlock">
            <v-icon>mdi-plus</v-icon>
          </v-btn>
            </v-row>
          <v-list dense class="rounded-lg">
            <v-list-item
              v-for="(block, index) in taskBlocks"
              :key="index"
              @click="selectBlock(index)"
              :class="{ 'selected-task': selectedBlock === index }"
              class="mb-2"
              style="margin:10px !important"
            >
              <v-list-item-content >
               <v-row
               style="display: flex !important;
               flex-direction: row !important;
               justify-content: space-between !important;
               align-items: center !important;
               padding-right: 15px;
               "
               >
               <v-list-item>{{ block.title }}</v-list-item>
    
                  <v-icon @click.stop="deleteBlock(index)" color="red">mdi-delete</v-icon>
              
                </v-row>
              </v-list-item-content>
            </v-list-item>
            
          </v-list>
        </v-col>
  
        <!-- Coluna para as tarefas do bloco selecionado -->
        <v-col cols="12" md="9" class="pa-2">
            <v-card-title style="display: flex;flex-direction: row;justify-content: space-between;align-items: center;">   {{ selectedTitle }}  <v-icon size="22" @click.stop="deleteBlock(index)" color="red">mdi-delete</v-icon>
             </v-card-title>
             
          <v-card class="task-container pa-4" flat height="400" style="overflow-y: scroll;">
          
  
            <!-- Lista de tarefas do bloco selecionado -->
            <v-list dense>
              <v-list-item
                v-for="(task, index) in selectedTasks"
                :key="index"
                class="task-item mb-2"
              >
                <v-list-item-content>
                  <v-list-item-title>
                   
                   
                      <v-icon  @click="deleteTask(index)" color="gray">mdi-delete</v-icon>
                      {{ task }}
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-row class="justify-space-between" style="
              background-color: #212121;padding: 10px;
              border-top-left-radius: 10px;
              border-top-right-radius: 10px;
              position: fixed; bottom: 0; width:70vw !important; align-items: center;">
              <v-text-field
          
                v-model="newTask"
                placeholder="Digite uma nova tarefa..."
                @keyup.enter="addTask"
                variant="outlined"
                dense
               
              ></v-text-field>
              <v-btn
              style="
           
              margin-bottom: 25px;
              margin-left: 10px;
              border-radius: 10px;
          
              height: 50px !important;
              "  color="primary" @click="addTask">
                <v-icon>mdi-plus</v-icon>
              </v-btn>
            </v-row>
            </v-list>

          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  export default {
    data() {
      return {
        selectedBlock: 0,
        newTask: '',
        taskBlocks: JSON.parse(localStorage.getItem('taskBlocks')) || [
          { title: 'Trabalho', tasks: ['Reunião com o time', 'Finalizar relatório'] },
          { title: 'Estudo', tasks: ['Ler capítulo 5', 'Fazer exercícios'] },
          { title: 'Escola', tasks: ['Entrega de trabalho de História', 'Revisar para prova'] },
          { title: 'Faculdade', tasks: ['Projeto de conclusão', 'Estudar para exame'] },
        ],
      };
    },
    computed: {
      selectedTitle() {
        return this.taskBlocks[this.selectedBlock]?.title || '';
      },
      selectedTasks() {
        return this.taskBlocks[this.selectedBlock]?.tasks || [];
      },
    },
    methods: {
      selectBlock(index) {
        this.selectedBlock = index;
      },
      addTask() {
        if (this.newTask) {
          this.taskBlocks[this.selectedBlock].tasks.push(this.newTask);
          this.newTask = '';
          this.saveBlocksToLocalStorage();
        }
      },
      deleteTask(index) {
        this.taskBlocks[this.selectedBlock].tasks.splice(index, 1);
        this.saveBlocksToLocalStorage();
      },
      addBlock() {
        this.taskBlocks.push({ title: 'Novo Bloco', tasks: [] });
        this.selectedBlock = this.taskBlocks.length - 1;
        this.saveBlocksToLocalStorage();
      },
      deleteBlock(index) {
        if (this.selectedBlock === index) {
          this.selectedBlock = 0;
        }
        this.taskBlocks.splice(index, 1);
        this.saveBlocksToLocalStorage();
      },
      saveBlocksToLocalStorage() {
        localStorage.setItem('taskBlocks', JSON.stringify(this.taskBlocks));
      },
    },
    watch: {
      taskBlocks: {
        deep: true,
        handler() {
          this.saveBlocksToLocalStorage();
        },
      },
    },
  };
  </script>
  
  <style scoped>
  .selected-task {
    background-color: #313131;

    border-radius: 8px !important;
    margin: 5px ;
  }
  .task-container {
  
    border-radius: 8px;
  }
  </style>
  