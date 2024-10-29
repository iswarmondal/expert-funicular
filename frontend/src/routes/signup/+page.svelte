<script lang="ts">
	import { pb } from '$lib/config/pocketbase.config';

	let email: string = '';
	let password: string = '';
    let passwordConfirm: string = '';
	let errorMessage: string = '';

	async function signUp() {
		try {
			errorMessage = "";
            if (password !== passwordConfirm) {
                errorMessage = "Password and confirm password are different"
                return;
            }
			const data = {
				email,
				emailVisibility: true,
				password,
				passwordConfirm: passwordConfirm,
				name: email.split('@')[0]
			};
			const user = await pb.collection('users').create(data);
			console.log('User created:', user);
			console.log(pb.authStore.isValid)
		} catch (error) {
			if (error instanceof Error) {
				errorMessage = error.message;
			} else {
				errorMessage = 'An unknown error occurred';
			}
			console.error('Error creating user:', error);
		}
	}
    </script>

<div class="flex justify-center items-center h-screen">
	<div class="card w-96 shadow-xl">
		<div class="card-body">
			<h2 class="card-title text-center">Sign Up</h2>
			<form on:submit|preventDefault={signUp} class="space-y-4">
				<div class="form-control">
					<label class="label" for="email">
						<span class="label-text">Email</span>
					</label>
					<input type="email" bind:value={email} class="input input-bordered w-full" required />
				</div>
				<div class="form-control">
					<label class="label" for="password">
						<span class="label-text">Password</span>
					</label>
					<input
						type="password"
						bind:value={password}
						class="input input-bordered w-full"
						required
					/>
				</div>
				<div class="form-control">
					<label class="label" for="confirm-password">
						<span class="label-text">Confirm Password</span>
					</label>
					<input
						type="password"
						bind:value={passwordConfirm}
						class="input input-bordered w-full"
						required
					/>
				</div>
				{#if errorMessage}
					<div class="text-red-500 text-sm">{errorMessage}</div>
				{/if}
				<div class="form-control mt-6">
					<button type="submit" class="btn btn-primary w-full">Sign Up</button>
				</div>
			</form>
		</div>
	</div>
</div>
