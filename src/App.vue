<template>
    <div class="jumbotron bg-primary text-light">
        <div class="container">
            <b>
                <h1 class="display-4">TODO 2</h1>
                <p class="lead">To do other verison.</p>
            </b>
        </div>
    </div>
    <br />
    <br />
    <div class="container">
        <div class="row">
            <!-- pomocou offset centruje bootstrap -->
            <div class="col-md-4 offset-3">
                <!-- keydown.enter na funkcnost enteru -->
                <!-- vmodel na zdielanie dat -->
                <input
                    type="text"
                    id="taskip"
                    @keydown.enter="insertItem"
                    v-model="todoItem"
                    placeholder="Add task here"
                    autofocus
                />
            </div>
        </div>
        <br />
        <br />
        <div class="row">
            <div class="col-md-12">
                <h3>Task list</h3>
                <table class="table table-striped">
                    <thead class="thead-dark">
                        <th>ID:</th>
                        <th>Name:</th>
                        <th>Is completed ?</th>
                        <th>Remove</th>
                    </thead>
                    <tbody>
                        <!-- preiterovanie polom todoList a ulozenie do task -->
                        <!-- vypln riadkov podla hodnoty -->
                        <tr v-for="(task, index) in todoList" :key="task">
                            <td>{{ task.id }}</td>
                            <td>
                                <!-- doubleclick listener na task pre editovanie-->
                                <!-- vbind nadstavi klasu ak task.isCompleted hodi true -->
                                <!-- ak task.editing je false zobrazi sa task.name inac input na editovanie -->
                                <div
                                    v-if="!task.editing"
                                    @dblclick="editTask(task)"
                                    v-bind:class="{
                                        completed: task.isCompleted,
                                    }"
                                >
                                    {{ task.name }}
                                </div>
                                <!-- input na upravovanie  -->
                                <!-- blur ak sa odklikne vtati zmeneny task.name a keydown.enter deto -->
                                <!-- editCompleted ymeni naspet task.editing na false -->
                                <input
                                    @blur="editCompleted(task)"
                                    @keydown.enter="editCompleted(task)"
                                    v-model="task.name"
                                    v-else
                                    type="text"
                                />
                            </td>
                            <td>
                                <!-- vmodel nadstavi hodnotu true alebo false -->
                                <input
                                    v-model="task.isCompleted"
                                    type="checkbox"
                                />
                            </td>
                            <td>
                                <!-- po kliknuti berie hodnotu index priamo z preiterovaneho pola -->
                                <button
                                    @click="removeItem(index)"
                                    class="btn btn-sm btn-danger"
                                >
                                    Remove
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "App",
    components: {},
    data() {
        return {
            //vlozene data cez input
            todoItem: "",
            //prednadstavene startovacie id
            id: 2,
            //ulozene data do pola
            todoList: [
                {
                    name: "Do homework",
                    id: 1,
                    editing: false,
                    isCompleted: false,
                },
            ],
        };
    },
    methods: {
        //nataha data do pola kde vytvori objekt s name, isCompleted a id
        //isCompleted je defaultne nadstaveny na false
        //id sa zvysi vzdy po pridani
        //nakoniec sa nadstavi input na prazdnu hodnotu
        insertItem() {
            console.log(this.todoItem);
            this.todoList.push({
                name: this.todoItem,
                isCompleted: false,
                id: this.id,
                editing: false,
            });
            this.id++;
            this.todoItem = "";
        },
        //metoda na editovanie tasku
        editTask(task) {
            task.editing = true;
        },
        //metoda na odkliknutie a odenterovanie editovaneho obsahu
        editCompleted(task) {
            task.editing = false;
        },
        //metoda na zmazanie z pola splice
        // pole.splice(cislo indexu, kolko zmazat);
        removeItem(index) {
            this.todoList.splice(index, 1);
        },
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
}
.jumbotron {
    background-color: rgb(11, 81, 117) !important;
    box-shadow: 0 40px 20px 0 rgba(0, 0, 0, 0.2);
}

#taskip {
    border: none;
    border-bottom: 1px solid lightgray;
    min-width: 70%;
    height: 3rem;
    text-align: center;
}
.completed {
    text-decoration: line-through;
    color: grey;
}
</style>
