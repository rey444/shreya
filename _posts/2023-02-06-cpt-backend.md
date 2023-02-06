---
title: Backend Step 1 - GET Inputs
layout: default
description: Supports grade inputs and calculates average. 
permalink: /frontend/grades
image: /images/grade_calc.png
categories: [2.C]
tags: [javascript, html, input, onblur]
week: 10
type: pbl
---

<!-- For hacks take inspiration form here: https://www.rapidtables.com/calc/grade/grade-calculator.html -->
<!-- Hack 1: change control flow to enable editing of previous element -->
<!-- Hack 2: build a title for each score -->

{% include nav_frontend.html %}

<html>
    <h3> 1. What type of car do you wish to buy? </h3>
        <input type="radio" id="html" name="carType" value="Sedan">
        <label for="Sedan">Sedan</label><br>
        <input type="radio" id="html" name="carType" value="SUV">
        <label for="SUV">SUV</label><br>
        <input type="radio" id="html" name="carType" value="Pickup Truck">
        <label for="Pickup Truck">Pickup Truck</label><br>
        <input type="radio" id="html" name="carType" value="Sports Car">
        <label for="Sports Car">Sports Car</label><br>
        <input type="radio" id="html" name="carType" value="Van">
        <label for="Van">Van</label><br>
        <input type="radio" id="html" name="carType" value="Convertible">
        <label for="Convertible">Convertible</label><br>
        <input type="radio" id="html" name="carType" value="Coupe">
        <label for="Coupe">Coupe</label><br>
    <h3> 2. How many people should your car be able to seat?</h3>
        <input type="radio" id="html" name="seatNumber" value="2">
        <label for="2">2</label><br>
        <input type="radio" id="html" name="seatNumber" value="5">
        <label for="5">5</label><br>
        <input type="radio" id="html" name="seatNumber" value="7">
        <label for="7">7</label><br>
        <input type="radio" id="html" name="seatNumber" value="8">
        <label for="8">8</label><br>
        <input type="radio" id="html" name="seatNumber" value="10">
        <label for="10">10</label><br>
        <input type="radio" id="html" name="seatNumber" value="15">
        <label for="15">15</label><br>
    <h3> 3. What power source do you prefer?</h3>
        <input type="radio" id="html" name="powerSource" value="Gasoline">
        <label for="Gasoline">Gasoline</label><br>
        <input type="radio" id="html" name="powerSource" value="Electric">
        <label for="Electric">Electric</label><br>
    <h3> 4. Transmission Type?</h3>
        <input type="radio" id="html" name="transmission" value="Automatic">
        <label for="Automatic">Automatic</label><br>
        <input type="radio" id="html" name="transmission" value="Manual">
        <label for="Manual">Manual</label><br>
    <h3> 5. Desired Mileage (in miles per gallon; please answer if you selected plug-in hybrid, hybird, gasoline, or diesel for question 3)</h3>
        <input type="radio" id="html" name="mileage" value="12-20 MPG">
        <label for="12-14 MPG">12-14 MPG</label><br>
        <input type="radio" id="html" name="mileage" value="21-30 MPG">
        <label for="21-30 MPG">21-30 MPG</label><br>
        <input type="radio" id="html" name="mileage" value="31-40 MPG">
        <label for="31-40 MPG">31-40 MPG</label><br>
        <input type="radio" id="html" name="mileage" value="41-50 MPG">
        <label for="41-50 MPG">41-50 MPG</label><br>
        <input type="radio" id="html" name="mileage" value="51-60 MPG">
        <label for="51-60 MPG">51-60 MPG</label><br>
    <h3> 6. Desired Range (in miles per charge; please answer if you selected electric for question 3)</h3>
        <input type="radio" id="html" name="range" value="100-150 Miles">
        <label for="100-150 Miles">100-150 Miles</label><br>
        <input type="radio" id="html" name="range" value="151-200 Miles">
        <label for="151-200 Miles">151-200 Miles</label><br>
        <input type="radio" id="html" name="range" value="201-250 Miles">
        <label for="201-250 Miles">201-250 Miles</label><br>
        <input type="radio" id="html" name="range" value="251-300 Miles">
        <label for="251-300 Miles">251-300 Miles</label><br>
        <input type="radio" id="html" name="range" value="301-350 Miles">
        <label for="301-350 Miles">301-350 Miles</label><br>
        <input type="radio" id="html" name="range" value="351-400 Miles">
        <label for="351-400 Miles">351-400 Miles</label><br>
        <input type="radio" id="html" name="range" value="401-450 Miles">
        <label for="401-450 Miles">401-450 Miles</label><br>
        <input type="radio" id="html" name="range" value="451-500 Miles">
        <label for="451-500 Miles">451-500 Miles</label><br>
    <button class="testbutton">Submit</button>
</html>

<style>
    .testbutton {
        background-color: white;
        border-radius: 8px;
        color: black;
        border: none;
        margin: 0;
        font-family: "Kanit", sans-serif;
        font-size: 20px;

    }

    .testbutton:hover {
        color: rgb(4, 4, 43);
    }

    label {
        font-family: "Kanit", sans-serif;
        font-size: 18px;
        color: white;
    }

    h3 {
        font-family: "Kanit", sans-serif;
        font-size: 20px;
        color: white;
    }

    h1 {
        font-family: "Kanit", sans-serif;
        font-size: 30px;
        color: white;
    }

    p {
        font-family: "Kanit", sans-serif;
        font-size: 15px;
        color: white;
    }
</style>

<script>
    const dataContainer = document.getElementById("data");

    // Creates new input line
    function newInputLine(index) {
        // Prepare new input line
        var input = document.createElement("input");  // input element
        var br = document.createElement("br");  // line break element
        // Setup input line attributes
        input.setAttribute('onblur', "calculator()");
        input.setAttribute('type', "text");
        input.setAttribute('name', "score");
        input.setAttribute('id', "score" + index);
        // Add input and line break to page
        scoresContainer.appendChild(input);
        scoresContainer.appendChild(br);
    }

    // Calculates totals
    function calculator(){
        var array = document.getElementsByName('data'); // setup array of data
        if (array[array.length-1].value.length != 0) {   // input cell has a value
            // algorithm to calculate results
            var total = 0;  // running total
            for(var i = 0; i < array.length; i++){  // iterate through array
                if(parseFloat(array[i].value))  // convert to float
                    total += parseFloat(array[i].value);  // add to running total
            }
            // update totals
            document.getElementById('total').innerHTML = total.toFixed(2);
            document.getElementById('count').innerHTML = array.length;
            document.getElementById('average').innerHTML = (total / array.length).toFixed(2);
            // make a new input line
            newInputLine(array.length);
            
        }
        // Set cursor focus on last element; this could be new or unchanged element
        document.getElementById("score" + (array.length-1)).focus();
    }

</script>