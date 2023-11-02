<template>
    <div class="hello">
        <!-- <p>{{ dataObject }}</p> -->
        <p>Time Zone: {{ dataObject.location?.tz_id }}</p>
        <p>Local Time: {{ dataObject.location?.localtime }}</p>
        <div class="card">
            <div>
                <img alt="cloud" :src="dataObject.current?.condition.icon"/>
            </div>
            <p>Condition: {{ dataObject.current?.condition.text }}</p>
            <p>Temperature (C): {{ dataObject.current?.temp_c }} </p>
            <p>Temperature (F): {{ dataObject.current?.temp_f }}</p>
        </div>
    </div>
</template>  
<script>

export default {
    name: 'WeatherView',
    props: {
        msg: String,
        dataObject: Object
    },
}
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
    margin: 40px 0 0;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: inline-block;
    margin: 0 10px;
}

a {
    color: #42b983;
}
</style>
  