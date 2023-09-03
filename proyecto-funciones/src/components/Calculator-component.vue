<template>
    <div class="h-screen bg-green-400 flex justify-center py-16">
      <div class="h-screen bg-green-400 flex justify-self-stretch py-16">
        <div class="w-1/2">

          <div class="grid grid-cols-1 gap-2 mt-10">

            <button class="h-16 w-40 text-xl text-white font-semibold bg-green-500 rounded">Calcular por</button>
            <button class="h-16 w-40 text-xl text-white font-semibold bg-green-500 rounded">Bisección</button>
            <button class="h-16 w-40 text-xl text-white font-semibold bg-green-500 rounded">Punto Fijo</button>
            <button class="h-16 w-40 text-xl text-white font-semibold bg-green-500 rounded">Newton Raphson</button>
            <button class="h-16 w-40 text-xl text-white font-semibold bg-green-500 rounded">Secante</button>
            <button class="h-16 w-40 text-xl text-white font-semibold bg-green-500 rounded">Gauss Seidel</button>
            <button class="h-16 w-40 text-xl text-white font-semibold bg-green-500 rounded">Jacobi</button>
            <button class="h-16 w-40 text-xl text-white font-semibold bg-green-500 rounded">Tropezoidal</button>
            <button class="h-16 w-40 text-xl text-white font-semibold bg-green-500 rounded">simpson</button>
        
          </div>
    
        </div>
      </div>

      <div class="w-1/2">

        <div class="p-4 text-right shadow-2xl rounded-t-lg bg-white overflow-hidden max-h-96">
          <p class="text-right text-gray-600 font-semibold">Resultado</p>
          <textarea readonly class="w-full h-full p-40 resize-none border border-gray-300 rounded"></textarea>
        </div>

        <div class="grid grid-cols-12 gap-2 mt-2">
        </div>

        <div class="p-4 text-right shadow-2xl rounded-t-lg bg-white">
          <p class="font-semibold text-gray-700 h-6">4556</p>
          <p class="font-bold text-3xl text-gray-900 h-6 my-2">0</p>
        </div>

        <div class="grid grid-cols-12 gap-2 mt-2">

          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">(</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">)</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">|</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">[</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">]</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
        
        </div>

        <div class="grid grid-cols-12 gap-2 mt-2">

          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">X</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">7</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">8</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">9</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">.</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">Ln</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">i</button>
        
        </div>

        <div class="grid grid-cols-12 gap-2 mt-2">

          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">Y</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">4</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">5</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">6</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">/</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">*</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">></button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">log</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
        
        </div>

        <div class="grid grid-cols-12 gap-2 mt-2">

          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">Z</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">1</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">2</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">3</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">-</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">+</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">  </button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
        
        </div>

        <div class="grid grid-cols-12 gap-2 mt-2">

          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">abc</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">,</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">0</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">.</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">%</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">--</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">=</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
          <button class="h-16 text-xl text-white font-semibold bg-green-500 rounded">AC</button>
        
        </div>

       

      </div>
    </div>
</template>

<script>
export default{
  
}
</script>
