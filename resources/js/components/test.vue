<template>
    <div class="relative p-2 flex overflow-x-auto h-full">


<!-- Columns (Statuses) -->
<div v-for="status in statuses" :key="status.slug" class="mr-6 w-4/5 max-w-xs flex-shrink-0">
  <div class="rounded-md shadow-md overflow-hidden">

    <!--Status Titles: Backlog, Done, etc-->
    <div class="p-3 flex justify-between items-baseline bg-blue-800 ">
      <h4 class="font-medium text-white">{{ status.title }}</h4>
      <button @click="openAddTaskForm(status.id)" class="py-1 px-2 text-sm text-orange-500 hover:underline">Add Task</button>
    </div>

    <!--Initialize list for sprints, userstories, task, etc.-->
    <div class="p-2 bg-blue-100">
      <!-- AddTaskForm -->
      <AddTaskForm :initial-data=status.task :sprint="sprints" :userstory="userstories" v-if="newTaskForStatus === status.id" :status-id="status.id" v-on:task-added="handleTaskAdded" v-on:task-canceled="closeAddTaskForm"/>
      <!-- ./AddTaskForm -->

      <!-- Tasks in the Status -->
      <draggable class="flex-1 overflow-hidden" v-model="status.tasks" v-bind="taskDragOptions" @end="handleTaskMoved">
        <transition-group class="flex-1 flex flex-col h-full overflow-x-hidden overflow-y-auto rounded shadow-xs" tag="div">
          <div v-for="task in status.tasks" :key="task.id" class="mb-3 p-4 flex flex-col bg-white rounded-md shadow transform hover:shadow-md cursor-pointer">
            <span class="block mb-2 text-xl text-gray-900">{{ task.title }}</span>
              <p class="text-gray-700 mb-3">{{ task.description }}</p>
              <p class="text-gray-700 mb-1"><span class="font-semibold text-black-900">Sprint :</span>{{ task.sprint_id }}</p>
              <p class="text-gray-700 mb-1"><span class="font-semibold text-black-900">User Story :</span>{{ task.u_id }}</p>
              <p class="text-gray-700 mb-1"><span class="font-semibold text-black-900">Start Date :</span>{{ task.start_date }}</p>
              <p class="text-gray-700 mb-1"><span class="font-semibold text-black-900">End Date :</span>{{ task.end_date }}</p>

            <!--Button to Edit or Delete the Task in the status-->
            <div class="p-3 flex justify-between items-end text-sm bg-gray-100">
              <button @click="openAddTaskForm(status.id)" class="px-3 py-1 leading-5 text-white bg-blue-600 hover:bg-blue-500 rounded">Edit</button>
              <button @click="openAddTaskForm(status.id)" class="px-3 py-1 leading-5 text-white bg-red-600 hover:bg-red-500 rounded">Delete</button>
           </div>
          </div>
          <!-- ./Tasks -->
        </transition-group>
      </draggable>

      <!-- If there is no tasks -->
      <div v-show="!status.tasks.length && newTaskForStatus !== status.id" class="flex-1 p-4 flex flex-col items-center justify-center">
        <span class="text-gray-600">No tasks yet</span>
        <button class="mt-1 text-sm text-orange-600 hover:underline" @click="openAddTaskForm(status.id)">Add one</button>
        <!-- <addtask-form :sprint="sprints" :userstory="userstories"></addtask-form> -->
      </div>
      <!-- ./No Tasks -->
    </div>
  </div>
</div>
<!-- ./Columns -->

</div>
</template>