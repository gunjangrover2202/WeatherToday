// Name- Gunjan Grover
//Student Name- 21355068
//Application Name- Weather Today
const express = require('express');
const app = express();
const cors = require("cors");
const fetch = (...args) => import('node-fetch').then(({ default: fetch }) => fetch(...args));

app.use(cors());
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
const port = 5000;
app.listen(port, () => {
    console.log(`Starting server at ${port}`);
});
 
app.get('/weather/:city', async (req, res) => {
    let city_Name = req.params.city;
    //Current weather API
    let lat = -1;
    let lon = -1;
    let weatherAPI = 'https://api.openweathermap.org/data/2.5/weather?q=' + city_Name +'&units=metric&appid=cc0e2539babd2164ec2b89991a8c013f';
    let w_res = await fetch(weatherAPI)
    let data_w = await w_res.json();
    // console.log(data_w);
    if(data_w.cod != 404){
     lat = data_w.coord.lat;
     lon = data_w.coord.lon;}
    //Forecast API
    let forecastAPI = 'https://api.openweathermap.org/data/2.5/forecast?q='+ city_Name +'&units=metric&appid=cc0e2539babd2164ec2b89991a8c013f';
    let f_res  = await fetch(forecastAPI);
    let data_f = await f_res.json();
    //Air pollution API
    let pollutionAPI = 'https://api.openweathermap.org/data/2.5/air_pollution?lat=' + lat +'&lon='+ lon +'&appid=cc0e2539babd2164ec2b89991a8c013f';
    let p_res  = await fetch(pollutionAPI);
    let data_p = await p_res .json();
    const data = {
        Weather: data_w,
        Forecast: data_f,
        Pollution: data_p
    };
    res.json(data);
});
