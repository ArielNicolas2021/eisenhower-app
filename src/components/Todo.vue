<template>
    <!-- Do first block -->
    <div class="first">
        <div class="first-content">
            <h2>COMPLETAR PRIMERO <span>({{ firstOptions.count }})</span></h2>
            <box-icon 
            :name="!firstOptions.collapsed ? 'chevron-down' : 'chevron-up'" 
            color="#fff"
            size="lg"
            @click="!firstOptions.collapsed ? firstOptions.collapsed = true : firstOptions.collapsed = false"></box-icon>
        </div>
        <div class="first-collapsed" :class="firstOptions.collapsed ? 'active' : ''">
            <div class="collapsed-tasks">
                <div class="task-block" v-for="(task, index) in taskList" :key="index">
                        <div class="task-block-content" v-if="task.important && task.urgent">
                        <box-icon
                            :name="!task.completed ? 'checkbox' : 'checkbox-checked'"
                            color="#3eebbe"
                            @click="handleFirstCompleted(task)">
                        </box-icon>
                        <input type="checkbox" :id="index">
                        <label 
                            :for="index"
                            @click="handleFirstCompleted(task)"
                            :style="!task.completed ? 'text-decoration:none' : 'text-decoration:line-through'"
                            :id="task.id">
                            {{ task.task }}
                        </label>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- /Do first block -->

    <!-- Schedule block -->
    <div class="schedule">
        <div class="schedule-content">
            <h2>PLANIFICAR <span>({{ scheduleOptions.count }})</span></h2>
            <box-icon 
            :name="!scheduleOptions.collapsed ? 'chevron-down' : 'chevron-up'" 
            color="#fff"
            size="lg"
            @click="!scheduleOptions.collapsed ? scheduleOptions.collapsed = true : scheduleOptions.collapsed = false"></box-icon>
        </div>
        <div class="schedule-collapsed" :class="scheduleOptions.collapsed ? 'active' : ''">
            <div class="collapsed-tasks">
                <div class="task-block" v-for="(task, index) in taskList" :key="index">
                    <div class="task-block-content" v-if="task.important && !task.urgent">
                        <box-icon
                            :name="!task.completed ? 'checkbox' : 'checkbox-checked'"
                            color="#FEC166"
                            @click="handleScheduleCompleted(task)">
                        </box-icon>
                        <input type="checkbox" :id="index">
                        <label 
                            :for="index"
                            @click="handleScheduleCompleted(task)"
                            :style="!task.completed ? 'text-decoration:none' : 'text-decoration:line-through'"
                            :id="task.id">
                            {{ task.task }}
                        </label>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- /Schedule block -->

    <!-- Delegate block -->
    <div class="delegate">
        <div class="delegate-content">
            <h2>DELEGAR <span>({{ delegateOptions.count }})</span></h2>
            <box-icon 
            :name="!delegateOptions.collapsed ? 'chevron-down' : 'chevron-up'"
            color="#fff"
            size="lg"
            @click="!delegateOptions.collapsed ? delegateOptions.collapsed = true : delegateOptions.collapsed = false"></box-icon>
        </div>
        <div class="delegate-collapsed" :class="delegateOptions.collapsed ? 'active' : ''">
            <div class="collapsed-tasks">
                <div class="task-block" v-for="(task, index) in taskList" :key="index">
                    <div class="task-block-content" v-if="!task.important && task.urgent">
                        <box-icon
                            :name="!task.completed ? 'checkbox' : 'checkbox-checked'"
                            color="#0AA4E7"
                            @click="handleDelegateCompleted(task)">
                        </box-icon>
                        <input type="checkbox" :id="index">
                        <label 
                            :for="index"
                            @click="handleDelegateCompleted(task)"
                            :style="!task.completed ? 'text-decoration:none' : 'text-decoration:line-through'"
                            :id="task.id">
                            {{ task.task }}
                        </label>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- /Delegate block -->

    <!-- Eliminate block -->
    <div class="eliminate">
        <div class="eliminate-content">
            <h2>ELIMINAR <span>({{ eliminateOptions.count }})</span></h2>
            <box-icon 
            :name="!eliminateOptions.collapsed ? 'chevron-down' : 'chevron-up'" 
            color="#fff"
            size="lg"
            @click="!eliminateOptions.collapsed ? eliminateOptions.collapsed = true : eliminateOptions.collapsed = false"></box-icon>
        </div>
        <div class="eliminate-collapsed" :class="eliminateOptions.collapsed ? 'active' : ''">
            <div class="collapsed-tasks">
                <div class="task-block" v-for="(task, index) in taskList" :key="index">
                    <div class="task-block-content" v-if="!task.important && !task.urgent">
                        <box-icon
                            :name="!task.completed ? 'checkbox' : 'checkbox-checked'"
                            color="#FF7387"
                            @click="handleEliminateCompleted(task)">
                        </box-icon>
                        <input type="checkbox" :id="index">
                        <label 
                            :for="index"
                            @click="handleEliminateCompleted(task)"
                            :style="!task.completed ? 'text-decoration:none' : 'text-decoration:line-through'"
                            :id="task.id">
                            {{ task.task }}
                        </label>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- /Eliminate block -->

    <!-- Add task btn -->
    <div class="add-task-btn" @click="!collapseForm ? collapseForm = true : collapseForm  = false">
        <box-icon name='plus' color="#fff"></box-icon>
    </div>
    <!-- /Add task btn -->

    <!-- Add task form -->
    <form class="add-task-form" v-if="collapseForm" @submit="addTask">
        <box-icon name='x' color="#fff" @click="collapseForm ? collapseForm = false : collapseForm = true"></box-icon>
        <h1>Nueva tarea</h1>
        <div class="form-input">
            <label for="task">Tarea</label>
            <input type="text" class="task" id="task" placeholder="Enter task here" autocomplete="off" ref="task">
        </div>
        <div class="category-block">
            <div class="important" :style="important ? 'background: #4EC1F3; color: #000;' : ''" @click="!important ? important = true : important = false">
                <span>IMPORTANTE</span>
            </div>
            <div class="urgent" :style="urgent ? 'background: #4EC1F3; color: #000;' : ''" @click="!urgent ? urgent = true : urgent=false">
                <span>URGENTE</span>
            </div>
        </div>
        <div class="btn-submit">
            <button type="submit">Agregar</button>
        </div>
    </form>
</template>

<script>
    export default {
        created() {
            /* First block */
            this.newTaskList = JSON.parse(localStorage.getItem('tasks'))
            this.firstCount = JSON.parse(localStorage.getItem('firstCount'))
            this.scheduleCount = JSON.parse(localStorage.getItem('scheduleCount'))
            this.delegateCount = JSON.parse(localStorage.getItem('delegateCount'))
            this.eliminateCount = JSON.parse(localStorage.getItem('eliminateCount'))
            if (this.newTaskList != null) {
                this.taskList = this.newTaskList
                this.firstOptions.count = this.firstCount
                this.scheduleOptions.count = this.scheduleCount
                this.delegateOptions.count = this.delegateCount
                this.eliminateOptions.count = this.eliminateCount
            }
        },
        data() {
            return {
                taskList: [],
                collapseForm: false,
                important: false,
                urgent: false,
                firstOptions: {
                    collapsed: false,
                    count: 0
                },
                scheduleOptions: {
                    collapsed: false,
                    count: 0
                },
                delegateOptions: {
                    collapsed: false,
                    count: 0
                },
                eliminateOptions: {
                    collapsed: false,
                    count: 0
                },
            }
        },
        methods: {
            addTask(e) {
                e.preventDefault();
                if(this.$refs.task.value.trim() != '') {
                    this.taskList.push({
                        id: Date.now(),
                        task: this.$refs.task.value,
                        important: this.important,
                        urgent: this.urgent,
                        completed: false,
                    })
                    if (this.important && this.urgent) {
                        this.firstOptions.count++
                        localStorage.setItem('firstCount', JSON.stringify(this.firstOptions.count))
                    } else if (this.important && !this.urgent) {
                        this.scheduleOptions.count++
                        localStorage.setItem('scheduleCount', JSON.stringify(this.scheduleOptions.count))
                    } else if (!this.important && this.urgent) {
                        this.delegateOptions.count++
                        localStorage.setItem('delegateCount', JSON.stringify(this.delegateOptions.count))
                    } else {
                        this.eliminateOptions.count++
                        localStorage.setItem('eliminateCount', JSON.stringify(this.eliminateOptions.count))
                    }
                    this.$refs.task.value = ''
                    this.selectedImportant = ''
                    this.selectedUrgent = ''
                    this.important = false
                    this.urgent = false
                    this.collapseForm = false
                    localStorage.setItem('tasks', JSON.stringify(this.taskList))
                } else {
                    alert('Please, add a task to do!')
                }
            },
            handleFirstCompleted(task) {
                if (!task.completed) {
                    task.completed = true
                    this.firstOptions.count--
                } else {
                    task.completed = false
                    this.firstOptions.count++
                }
                localStorage.setItem('firstCount', JSON.stringify(this.firstOptions.count))
                localStorage.setItem('tasks', JSON.stringify(this.taskList))
            },
            handleScheduleCompleted(task) {
                if (!task.completed) {
                    task.completed = true
                    this.scheduleOptions.count--
                } else {
                    task.completed = false
                    this.scheduleOptions.count++
                }
                localStorage.setItem('scheduleCount', JSON.stringify(this.scheduleOptions.count))
                localStorage.setItem('tasks', JSON.stringify(this.taskList))
            },
            handleDelegateCompleted(task) {
                if (!task.completed) {
                    task.completed = true
                    this.delegateOptions.count--
                } else {
                    task.completed = false
                    this.delegateOptions.count++
                }
                localStorage.setItem('delegateCount', JSON.stringify(this.delegateOptions.count))
                localStorage.setItem('tasks', JSON.stringify(this.taskList))
            },
            handleEliminateCompleted(task) {
                if (!task.completed) {
                    task.completed = true
                    this.eliminateOptions.count--
                } else {
                    task.completed = false
                    this.eliminateOptions.count++
                }
                localStorage.setItem('eliminateCount', JSON.stringify(this.eliminateOptions.count))
                localStorage.setItem('tasks', JSON.stringify(this.taskList))
            }
        }
    }
</script>