
# Vuejs Nano Coin payment processor

The component aims to implement a simple and immediate way to accept
payments in nano coin. This will allow an easier implementation
and a faster development of the idea without the need to understand
the underlying process.

## Authors

- [@sebo_the_tramp](https://github.com/Sebo-the-tramp/)

## Acknowledgements

- [NANO Trentino Alto Adige](https://github.com/nanotaa) for the code example
- [James Coxon](https://github.com/jamescoxon) for the implementation and hosting of the confirmation server

## Usage/Examples

```javascript
import PaymentComponent from "nano-payment-vue";

...

components: {   
    PaymentComponent,
    [...]    
  },


...


data() {
    return {      
      is_payment_confirmed: false,
      dest_address: "nano_1cuf6facdwf65mtwijq1h16rcig7k9qodj9qch6afuffgg78zumzeesu6e5z",
      amount: 0.0001,
    };
}

...

<payment-component v-model="is_payment_confirmed" :address="dest_address" :amount="amount"></payment-component>

```

__That's easy as that!__

Whenever the payment is funded, the value of _is_payment_confirmed_ is updated
and can trigger some action in the main component.
  
## Documentation

The component connects to a websocket server hosted at: [](wss://yapraiwallet.space/call).

It sends the address that has to be monitored.

Whenever a new transaction is authorized, the websocket update all the listeners.

If the payed sum is equal or more than the due amount, the boolean _is_payment_confirmed_ is set to true.

## Roadmap

- Fix Google chrome bug

- Add nice UX
  
## License

[MIT](https://choosealicense.com/licenses/mit/)
