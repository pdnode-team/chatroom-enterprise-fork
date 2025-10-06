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
		if (pb.authStore.isValid) {
			window.location.href = '/';
		}
	});

	let email = $state('');
	let password = $state('');
	let name = $state('');
	let confirmPassword = $state('');
	let error = $state('');
	let disabled = $state(false);

	async function handleSubmit(event: Event) {
		disabled = true;
		event.preventDefault();
		// Add your form submission logic here
		if (!email || !password || !confirmPassword || !name) {
			error = 'All fields are required!';
			disabled = false;
			return;
		} else if (password !== confirmPassword) {
			error = 'Passwords do not match!';
			disabled = false;
			return;
		} else if (password.length < 6) {
			error = 'Password must be at least 6 characters long!';
			disabled = false;
			return;
		} else if (!isValidEmail(email)) {
			error = 'Please enter a valid email address!';
			disabled = false;
			return;
		}

		try {
			const record = await pb.collection('users').create({
				email,
				emailVisibility: true,
				name,
				password,
				passwordConfirm: confirmPassword
			});
			await pb.collection('users').requestVerification(email);
			console.log(record);
			window.location.href = '/';
		} catch (err: Error | unknown) {
			error = err instanceof Error ? err.message : String(err);
			disabled = false;
			return;
		}

		// if (authError) {
		// 	error = authError.message;
		// 	disabled = false;
		// 	return;
		// } else {
		// 	error = 'Registration successful! Please check your email to verify your account.';
		// }
	}

	function isValidEmail(email: string): boolean {
		// 一个常用的、用于验证大部分有效邮箱地址的正则表达式
		const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

		return emailRegex.test(email);
	}
</script>

<svelte:head>
	<title>Pdnode Chat | Register</title>
</svelte:head>
<div class="flex min-h-screen items-center justify-center bg-gray-50 p-4 sm:p-6 lg:p-8">
	<div class="w-full max-w-md space-y-8 rounded-xl bg-white p-8 shadow-xl md:p-10">
		<div class="text-center">
			<h1 class="text-3xl font-bold tracking-tight text-gray-900">Create Your Account</h1>
			<p class="mt-2 text-sm text-gray-600">
				Already registered?
				<a href="/login" class="font-medium text-indigo-600 hover:text-indigo-500"> Sign In </a>
			</p>
		</div>

		<form class="space-y-6">
			<div>
				<label for="name" class="mb-1 block text-sm font-medium text-gray-700"> Name </label>
				<input
					id="name"
					name="name"
					type="text"
					bind:value={name}
					required
					placeholder="Your Name"
					class="block w-full rounded-md border border-gray-300 px-3 py-2 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
				/>
			</div>
			<div>
				<label for="email" class="mb-1 block text-sm font-medium text-gray-700">
					Email Address
				</label>
				<input
					id="email"
					name="email"
					type="email"
					bind:value={email}
					required
					placeholder="you@example.com"
					class="block w-full rounded-md border border-gray-300 px-3 py-2 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
				/>
			</div>

			<!-- Password Input Group -->
			<div>
				<label for="password" class="mb-1 block text-sm font-medium text-gray-700">
					Password
				</label>
				<input
					id="password"
					name="password"
					type="password"
					bind:value={password}
					required
					placeholder="Enter your password"
					class="block w-full rounded-md border border-gray-300 px-3 py-2 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
				/>
			</div>

			<!-- Confirm Password Input Group -->
			<div>
				<label for="confirm-password" class="mb-1 block text-sm font-medium text-gray-700">
					Confirm Password
				</label>
				<input
					id="confirm-password"
					name="confirm-password"
					type="password"
					bind:value={confirmPassword}
					required
					placeholder="Confirm your password"
					class="block w-full rounded-md border border-gray-300 px-3 py-2 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
				/>
			</div>
			<p class="mt-2 text-sm text-red-600">{error}</p>

			<!-- Submit Button -->
			<button
				type="submit"
				onclick={handleSubmit}
				{disabled}
				class="flex w-full justify-center rounded-md border border-transparent bg-indigo-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-indigo-700 focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 focus:outline-none disabled:bg-gray-400"
			>
				Register
			</button>
		</form>
	</div>
</div>
