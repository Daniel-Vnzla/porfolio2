<script>
	import { onDestroy } from 'svelte';
	import emailjs from 'emailjs-com';
	import Button from '../common/Button.svelte';
	import Line from '../common/Line.svelte';

	let values = {
		name: "",
		email: "",
		subject: "",
		message: "",
	}

	let messageStatus = "";
	let messageTime = null;

	const sendEmail = async () => {
		messageStatus = "";
		const templateParams = {
	    name: values.name,
	    email: values.email,
			subject: values.subject,
	    message: values.message,
		};
	   emailjs.send(__process.env.SERVICE_ID,__process.env.TEMPLATE_ID, templateParams, __process.env.USER_ID)
	    .then(({ status, text }) => {
	    	console.log('SUCCESS!', status, text);
	    	messageStatus = "SENT";

    		values = {
					name: "",
					email: "",
					subject: "",
					message: "",
				}
	    })
	    .catch((err) => {
	    	console.log('FAILED...', err)
	    	messageStatus = 'ERROR';
	    })
	    .finally(() => {
	    	 messageTime = setTimeout(() => messageStatus = "", 2000);
	    });
		}
 
 	onDestroy(() => clearTimeout(messageTime))

</script>

<div class="form">
		<h2 class="title">Contact</h2>
		<div class="line-right">
			<Line
			  time="var(--transition-time)"
			  color="var(--contact-color)" 
				distance="100%" 
				position="left" 
				origin="top" 
				volumen="4px" delay="calc(1.2 * var(--transition-time))" />
		</div>
	<form on:submit|preventDefault={sendEmail}>
		<Line
		  time="var(--transition-time)"
		  color="var(--contact-color)" 
			distance="100%" 
			position="top" 
			origin="center" 
			volumen="4px" delay="calc(2 * var(--transition-time))" />
		<Line
		  time="var(--transition-time)"
		  color="var(--contact-color)" 
			distance="100%" 
			position="left" 
			origin="top" 
			volumen="4px" delay="calc(2.8 * var(--transition-time))" />
		<Line
		  time="var(--transition-time)"
		  color="var(--contact-color)" 
			distance="102px" 
			position="bottom" 
			origin="left" 
			volumen="4px" delay="calc(3.6 * var(--transition-time))" />
		<Line
		  time="var(--transition-time)"
		  color="var(--contact-color)" 
			distance="114px" 
			position="right" 
			origin="top" 
			volumen="4px" delay="calc(2.8 * var(--transition-time))" />

		<div class="custom-input name">
			<span class="line-input">
				<Line
				  time="var(--transition-time)"
				  color="var(--contact-color)" 
					distance="100%" 
					position="bottom" 
					origin="left" 
					volumen="4px" delay="calc(2.8 * var(--transition-time))" />
			</span>
			<input type="text" name="name" bind:value={values.name} placeholder="Name" required />
		</div>
		<div class="custom-input email">
			<input 
				type="email" 
				name="email" 
				bind:value={values.email} 
				placeholder="Email" 
				required />
		</div>
		<div class="custom-input subject">
			<span class="line-input">
				<Line
				  time="var(--transition-time)"
				  color="var(--contact-color)" 
					distance="100%" 
					position="bottom" 
					origin="left" 
					volumen="4px" delay="calc(3 * var(--transition-time))" />
			</span>
			<input type="text" name="subject" bind:value={values.subject} placeholder="Subject" required />
		</div>
		<div class="custom-input message">
			<span class="line-input">
				<Line
				  time="var(--transition-time)"
				  color="var(--contact-color)" 
					distance="100%" 
					position="bottom" 
					origin="left" 
					volumen="4px" delay="calc(3.2 * var(--transition-time))" />
			</span>
			<textarea type="text" name="message" bind:value={values.message} placeholder="Message" required />
		</div>
		<div class="button">
			<Button label="Send email" size="25px" color="var(--contact-color)"/>
		</div>
	</form>
</div>

{#if messageStatus === "SENT"}
	<div class="form-message correct">
		Message sended
	</div>
{:else if messageStatus === "ERROR"}
	<div class="form-message error">
		Error when trying to send mail
	</div>
{/if}

<style>
	.form {
		position: relative;
	}

	.form form {
		position: relative;
		padding: 40px;
		display: grid;
		grid-gap: 15px;
		grid-template-columns: repeat(2,1fr);
	}

	.form .title {
		text-align: center;
		width: 80%;
		margin: 0 auto;
		color: #fff;
		font-size: var(--title-size);
		border-bottom: 1px solid var(--contact-color);
	}

	.form .title::first-letter {
		font-size: 5rem;
		color: var(--contact-color);
	}

	.line-right {
		position: relative;
		width: 4px;
		height: 50px;
		margin: 0 auto;
	}

	.form .custom-input {
		position: relative;
		opacity: 0;
		animation: vanish var(--transition-time) ease-in-out forwards;
		animation-delay: 1s;
	}

	.line-input {
		position: absolute;
		top: 50%;
		left: 0;
		width: 25px;
		height: 4px;
		margin-left: -38px;
		transform: translateY(-50%)
	}

	.form .custom-input textarea,
	.form .custom-input input {
		outline: none;
		padding: 10px 20px;
		font-size: 1.3rem;
		width: 100%;
		background: #a993ec9e;
		border: none;
		border-radius: var(--border-radius);
		color: #fff;
	}

	.form .custom-input textarea {
		resize: none;
		height: 150px;
		margin-bottom: 20px;
	}

	::placeholder {
		color: rgba(255,255,255, .7);
	}

	.form .message,
	.form .subject {
		grid-column: 1 / 3;
	}

	.form .button {
		position: absolute;
		bottom: -30px;
		left: 100px;
		z-index: 5;
		opacity: 0;
		animation: vanish var(--transition-time) ease-in-out forwards;
		animation-delay: 1800ms;
	}

	.message .line-input {
		top: 20px;
		transform: translateY(0);
	}

	.form-message {
		position: fixed;
    top: 100px;
    left: 0;
    z-index: 100;
    padding: 5px 20px;
    border: var(--border-radius);
    font-weight: bold;
    margin-left: 5px;
    transform: translateX(-200%);
    animation: translate var(--transition-time) linear forwards;
	}

	.correct {
		background: #00d424;
	}

	.error { 
		background: #d40000;
	}

	@keyframes translate {
		from{
    	transform: translateX(-200%);
		}
		to {
    	transform: translateX(0);
		}
	}

	@keyframes vanish {
		from {
			transform: translate(-5%);
			opacity: 0;
		}
		to {
			transform: translate(0);
			opacity: 1;
		}
	}


	@media (max-width: 520px) {
		.form form {
			display: flex;
			flex-direction: column;
		}
	}
</style>