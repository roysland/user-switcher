<script>
    import { createEventDispatcher } from "svelte";
    let userInput = {
        email: "",
        password: "",
        role: "",
    };
    let users
    const dispatch = createEventDispatcher();
    const getUsers = () => {
        const existing = window.localStorage.getItem("users")
        if (!existing) {
            window.localStorage.setItem("users", JSON.stringify([]))
        } else {
            return JSON.parse(existing)
        }
    }


    const saveUser = () => {
        const storedUsers = JSON.parse(window.localStorage.getItem("users"))
        storedUsers.push(userInput)
        window.localStorage.setItem("users", JSON.stringify(storedUsers))
        dispatch("userAdded", userInput)
    };
</script>

<div class="p-4">
    <div>
        <label
            for="email"
            class="block text-sm font-medium leading-6 text-gray-900"
            >Email</label
        >
        <div class="mt-2">
            <input
                type="email"
                name="email"
                bind:value={userInput.email}
                id="email"
                class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                placeholder="you@example.com"
            />
        </div>
    </div>

    <div>
        <label
            for="password"
            class="block text-sm font-medium leading-6 text-gray-900"
            
            >Password</label
        >
        <div class="mt-2">
            <input
                type="password"
                name="password"
                id="password"
                bind:value={userInput.password}
                class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                placeholder="you@example.com"
            />
        </div>
    </div>
    <div>
        <label for="email" class="block text-sm font-medium leading-6 text-gray-900">Role</label>
        <div class="mt-2">
          <input type="text" bind:value={userInput.role} name="text" id="text" class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" placeholder="Administrator">
        </div>
    </div>
</div>

<div class="flex items-center justify-end p-4">
    <button
        on:click={saveUser}
        class="pointer-events-auto rounded-md bg-red-600 px-3 py-2 text-[0.8125rem] font-semibold leading-5 text-white hover:bg-indigo-500"
    >
        Add User
    </button>
</div>
