---
layout: post
title: Hướng dẫn gradient descent cho người mới bắt đầu - Tutorial simple Gradient Descent for beginer
---

### Giới thiệu
<div id="training-one-chart" class="training-chart"/>
<table id="training-one" class="training-table">
    <tr>
        <td>
            Error
        </td>
        <td colspan="2">
            <span id="error-value" ></span>
        </td>
    </tr>
    <tr>
        <td class="error-cell" colspan="3">
            <span id="error-value-message"></span>&nbsp;
        </td>
    </tr>
    <tr>
        <td>
            Weight
        </td>
        <td>
            <input id="weightSlider" type="range" class="weight" min="0" max="0.4" step="0.001"
                >
        </td>
        <td class="slider-value">
            <span id="weight" class="weight">0</span>
        </td>
    </tr>
    <tr>
        <td>
            Bias
        </td>
        <td>
            <input id="biasSlider" type="range" class="bias"  min="0" max="460" step="1" >
        </td>
        <td class="slider-value">
            <span id="bias" class="bias">0</span>
        </td>
    </tr>
</table>

<div id="neural-network-graph" class="nn-graph-area" ></div>
