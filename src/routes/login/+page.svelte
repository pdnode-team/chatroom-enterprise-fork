<script lang="ts">
	import { supabase } from '$lib/supabaseClient';

	$effect(() => {
		supabase.auth.getSession().then(({ data: { session } }) => {
			if (session) {
				window.location.href = '/';
			}
		});
	});

	let email = $state('');
	let password = $state('');
	let error = $state('');
	let disabled = $state(false);

	async function handleSubmit(event: Event) {
		disabled = true;
		event.preventDefault();
		// Add your form submission logic here
		if (!email || !password) {
			error = 'All fields are required!';
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

		const { error: authError } = await supabase.auth.signInWithPassword({
			email,
			password
		});

		if (authError) {
			error = authError.message;
			disabled = false;
			return;
		} else {
			error = 'Login successful!';
			window.location.href = '/';
		}
	}

	function isValidEmail(email: string): boolean {
		// 一个常用的、用于验证大部分有效邮箱地址的正则表达式
		const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

		return emailRegex.test(email);
	}
</script>

<svelte:head>
	<title>Pdnode Chat | Login</title>
</svelte:head>
<div class="flex min-h-screen items-center justify-center bg-gray-50 p-4 sm:p-6 lg:p-8">
	<div class="w-full max-w-md space-y-8 rounded-xl bg-white p-8 shadow-xl md:p-10">
		<div class="text-center">
			<h1 class="text-3xl font-bold tracking-tight text-gray-900">Login Your Account</h1>
			<p class="mt-2 text-sm text-gray-600">
				Don't have an account?
				<a href="/register" class="font-medium text-indigo-600 hover:text-indigo-500"> Sign Up </a>
			</p>
		</div>

		<form class="space-y-6">
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

			<p class="mt-2 text-sm text-red-600">{error}</p>

			<!-- Submit Button -->
			<button
				type="submit"
				onclick={handleSubmit}
				{disabled}
				class="flex w-full justify-center rounded-md border border-transparent bg-green-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-green-700 focus:ring-2 focus:ring-green-500 focus:ring-offset-2 focus:outline-none disabled:bg-gray-400"
			>
				Login
			</button>
		</form>
	</div>
</div>
