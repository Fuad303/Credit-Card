<script>
	import Inputmask from 'inputmask'
	import creditCardType from 'credit-card-type'

let backface = false,
	numberInput,
	expireInput,
	cvvInput
let card = {
	number: '',
	name: '',
	expire: '',
	cvv: ''
}
$: type = card.number ? creditCardType(card.number)?.[0]?.type : false

$: if(numberInput && expireInput && cvvInput){
	Inputmask({
		'mask': '9999 9999 9999 9999',
		'placeholder': '',

	}).mask(numberInput)
	Inputmask({
		'mask': '99/99',
		'placeholder': '',

	}).mask(expireInput)
	Inputmask({
		'mask': '999',
		'placeholder': '',

	}).mask(cvvInput)
}

</script>

<div class="card" class:flip={backface}>
	<div class="front">
		<div class="card-top">
			<img src="img/Chip.svg" alt="">
			{#if type}
			<img class="card-type" src="img/{type}.svg" alt="">
			{/if}
		</div>
		<div class="card-number">
			{card.number || '**** **** **** ****'}
		</div>
		<div class="card-bottom">
			<div>
				<div class="key">Card Holder Name</div>
				<div class="value card-name">{card.name || "-"}</div>
			</div>
			<div>
				<div class="key">Expiry Date</div>
				<div class="value">{card.expire || "**/**"}</div>
			</div>
		</div>
	</div>
	<div class="back">
		<div class="card-back">
			CVV <em>{card.cvv || "***"}</em>
		</div>
	</div>
</div>
<input type="text"  bind:this={numberInput} bind:value={card.number} placeholder="Number">
<input type="text" bind:value={card.name} placeholder="Name">
<input type="text" bind:this={expireInput} bind:value={card.expire} placeholder="End date">
<input type="text" bind:this={cvvInput} on:focus={() => backface = true} on:blur={() => backface = false} bind:value={card.cvv}  placeholder="CVV">
<style>
	.card{
		width: 352px;
		height: 223px;
		position: relative;
		perspective: 800px;
		margin-bottom: 15px;
	}
	 .front, .back{
		width: inherit;
		height: inherit;
		background-color: #4285F4;
		border-radius: 15px;
		position: absolute;
		top: 0;
		left: 0;
		backface-visibility: hidden;
		transition: 1s all;
		padding: 31px 27px;
		display: flex;
		flex-direction: column;
	}
	.back{
		transform: rotateY(180deg);
	}
	.card.flip .back{
		transform: rotateY(0);
	}
	.card.flip .front{
		transform: rotateY(-180deg);
	}
	.card .front .card-top{
		display: flex;
		justify-content: space-between;
	}
	.card .front .card-bottom{
		margin-top: auto;
		display: flex;
		justify-content: space-between;
		color: #FFFFFF;
	}
	.card .front .card-bottom .key{
		font-size: 12px;
		font-weight: 500;
		text-transform: uppercase;
		opacity: 0.7;
		margin-bottom: 4px;
	}
	.card .front .card-bottom .value{
		font-size: 14px;
		font-weight: 600;
	}
	.card .front .card-number{
		font-size: 25px;
		font-family: monospace;
		margin-top: 20px;
		color: #FFFFFF;
	}
	.card .back .card-back{
		background-color: #FFFFFF;
		color: #000;
		padding: 10px;
		margin-top: 20px;
	}
	.card .back .card-back em{
		font-weight: 600;
		margin-left: 10px;
	}
	.card .front .card-bottom .card-name{
		text-transform: capitalize;
	}
	.card .front .card-type{
		width: 60px;
		height: 50px;
	}
</style>