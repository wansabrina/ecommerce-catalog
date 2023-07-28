<template>
    <div class="loader-wrapper">
      <div class="loader"></div>
    </div>
  </template>
  
  <style>
  .loader-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100px;
  }
  
  .loader {
    border: 10px solid rgba(0, 0, 0, 0.3);
    border-top: 10px solid #1E1E1E;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    animation: spin 2s linear infinite;
  }
  
  @keyframes spin {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
  </style>
  