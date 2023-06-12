<script>
    import UserAdd from "./UserAdd.svelte";

    let showAddUser = false;
    let getCurrentUser = localStorage.getItem("currentUser");
    

    const getUsers = () => {
        return JSON.parse(window.localStorage.getItem("users"))
    }
    
    const mockAuth = async (username, password) => {
        const req = await fetch("https://nav.localhost:4001/auth/credentials", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify({
                UserName: username,
                Password: password
            }),
        });

        const res = await req.json();
        console.log(res)
        return res
    }

    const switchUser = async (user) => {
        localStorage.setItem("currentUser", user.email);
        getCurrentUser = user.email;
        const login = await mockAuth(user.email, user.password)
        let [tab] = await chrome.tabs.query({ active: true, currentWindow: true });
        console.log(tab)
        chrome.cookies.set({
            name: 'ss-id',
            value: login.SessionId,
            url: tab.url,
        })
        refreshList()
    };

    const deleteAll = () => {
        window.localStorage.setItem("users", JSON.stringify([]))
        refreshList()
    }

    const userCreateToggle = () => {
        showAddUser = !showAddUser;
        getUsers()
    };

    const refreshList = () => {
        users = getUsers()
        showAddUser = false
    }

    let users = getUsers()
</script>

<div
    class="w-[24.5rem] divide-y divide-slate-400/20 rounded-lg bg-white text-[0.8125rem] leading-5 text-slate-900 ring-1 ring-slate-700/10"
>
{#if !showAddUser}
    {#each users as user}
    <div class="flex items-center p-4">
        <div class="ml-4 flex-auto">
            <div class="font-medium">{user.email}</div>
            <div class="mt-1 text-slate-700">@{user.role}</div>
        </div>
        {#if user.email !== getCurrentUser}
        <button
            type="button"
            on:click={() => switchUser(user)}
            class="pointer-events-auto ml-4 flex-none rounded-md px-2 py-[0.3125rem] font-medium text-slate-700 shadow-sm ring-1 ring-slate-700/10 hover:bg-slate-50"
        >
            Switch
        </button>
        {/if}
    </div>
    {/each}
    {/if}
    {#if showAddUser}
        <UserAdd on:userAdded={refreshList} />
    {/if}

    <div class="p-4 flex items-center justify-between">
        {#if users.length > 0}
        <button class="pointer-events-auto ml-8 rounded-md bg-red-600 px-3 py-2 text-[0.8125rem] font-semibold leading-5 text-white hover:bg-red-500" on:click={deleteAll}>
            Slett alle
        </button>
        {/if}
        <button
            on:click={() => (showAddUser = !showAddUser)}
            class="pointer-events-auto ml-8 rounded-md bg-indigo-600 px-3 py-2 text-[0.8125rem] font-semibold leading-5 text-white hover:bg-indigo-500"
        >
            {#if showAddUser}
                Cancel
            {:else}
                Add User
            {/if}
        </button>
    </div>
</div>
