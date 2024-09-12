<script>
import axios from "axios";

export default {
  data() {
    return {
      show: false,
    }
  },
  mounted() {
    this.getLanguage();
  },
  methods: {
    getLanguage() {
      let data = JSON.stringify({
        "language": "python",
        "version": "3.10.0",
        "files": [
          {
            "name": "my_code.py",
            "content": "print(3+2)"
          }
        ],
        "stdin": "150", 
        "args": "",
        "compile_timeout": 10000,
        "run_timeout": 3000,
        "compile_memory_limit": -1,
        "run_memory_limit": -1
      });

      let config = {
        method: 'post',
        maxBodyLength: Infinity,
        url: 'http://localhost:2000/api/v2/execute',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
        data: data
      };

      axios.request(config)
        .then((response) => {
          console.log(JSON.stringify(response.data));
        })
        .catch((error) => {
          console.log(error);
        });
    }
  }
}
</script>

<template>
  <div class="flex h-screen">
    <!-- Left Side (80% of the screen) -->
    <div class="w-4/5 p-4">

      <label for="message" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Kodingizni
        kiriting</label>
      <textarea id="message" rows="40"
        class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
        placeholder="Code..."></textarea>

    </div>

    <!-- Right Side (20% of the screen) -->
    <div class="w-1/5 p-4 flex flex-col">
      <!-- Input Field (Textarea) -->
      <textarea class="border border-gray-300 p-2 mb-4 shadow-lg rounded-lg h-1/3"
        placeholder="Enter input..."></textarea>

      <!-- Output Display -->
      <div class="flex-1 border border-gray-300 p-4 shadow-lg rounded-lg">
        Output will be displayed here...
      </div>
    </div>
  </div>
</template>