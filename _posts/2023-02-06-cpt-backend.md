---
title: Backend Step 1 - GET Inputs
layout: base
description: GET Inputs for CPT Project
permalink: /backend/cpt
categories: [t2]
tags: [t2]
---

<!-- For hacks take inspiration form here: https://www.rapidtables.com/calc/grade/grade-calculator.html -->
<!-- Hack 1: change control flow to enable editing of previous element -->
<!-- Hack 2: build a title for each score -->

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
    <h3> 5. Desired Mileage (in miles per gallon)</h3>
        <input type="radio" id="html" name="mileage" value="Non-Gasoline">
        <label for="Non-Gasoline">Non-Gasoline (select if you want an electric car)</label><br>
        <input type="radio" id="html" name="mileage" value="a">
        <label for="a">a</label><br>
        <input type="radio" id="html" name="mileage" value="b">
        <label for="b">b</label><br>
        <input type="radio" id="html" name="mileage" value="c">
        <label for="c">c</label><br>
    <h3> 6. Desired Range (in miles per charge)</h3>
        <input type="radio" id="html" name="mileage" value="Non-Electric">
        <label for="Non-Electric">Non-Electric (select if you want a gasoline car)</label><br>
        <input type="radio" id="html" name="mileage" value="1">
        <label for="1">1</label><br>
        <input type="radio" id="html" name="mileage" value="2">
        <label for="2">2</label><br>
        <input type="radio" id="html" name="mileage" value="3">
        <label for="3">3</label><br>
        <input type="radio" id="html" name="mileage" value="4">
        <label for="4">4</label><br>
    <button class="testbutton">Submit</button>
    <table class="table-latitude">
                <thead>
                    <tr>
                        <th>Type</th>
                        <th>Seating Capacity</th> 
                        <th>Power Source</th>
                        <th>Transmission Type</th>
                        <th>Mileage</th>
                        <th>Range</th>
                    </tr>
                    </thead>
                     <tbody id="result">
                    </tbody>
                </table>
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
        input.setAttribute('onblur', "calculator()"); //WIP: add in attributes for the 6 questions
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