## react-stripe-checkout
https://github.com/azmenak/react-stripe-checkout

with TS, I got this error

```
  <StripeCheckout
    name='Emaily'
    description='$5 for 5 email credits'
    amount={500}
    token={(token) => handleToken(token)}
    stripeKey={process.env['REACT_APP_STRIPE_KEY']}
  >
  
```

error
```
No overload matches this call.
  Overload 1 of 2, '(props: Readonly<StripeCheckoutProps>): StripeCheckout', gave the following error.
    Type 'string | undefined' is not assignable to type 'string'.
      Type 'undefined' is not assignable to type 'string'.
  Overload 2 of 2, '(props: StripeCheckoutProps, context?: any): StripeCheckout', gave the following error.
    Type 'string | undefined' is not assignable to type 'string'.
      Type 'undefined' is not assignable to type 'string'.ts(2769)
```


fix the error as below
```
stripeKey={process.env['REACT_APP_STRIPE_KEY'] || ''}

```

