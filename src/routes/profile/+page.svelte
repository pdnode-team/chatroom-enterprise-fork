<script lang="ts">
	/* 
	Chatroom Enterprise Fork
    Pdnode ChatRooM Enterprise Fork (C) 2025  PDNODE LLC

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <https://www.gnu.org/licenses/>.

	*/
	import { pb } from '$lib/pb';
	$effect(() => {
		if (!pb.authStore.isValid) {
			window.location.href = '/login';
		}
	});
</script>

<svelte:head>
	<title>Pdnode Chat | Profile</title>
</svelte:head>
<div class="flex h-screen w-full items-center justify-center bg-gray-100">
	<div class="w-full max-w-md rounded bg-white p-8 shadow-md">
		<h1 class="mb-6 text-center text-2xl font-bold">Profile</h1>
	</div>
</div>
