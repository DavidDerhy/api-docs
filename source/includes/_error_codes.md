# Errors Codes - WIP

We're trying to introduce unique error codes to make your lives even easier. Please bear with us, since we'll need some time to figure out the best approach.

For now we provide an additional `key` attribute along with the error response.

You can find some of the error codes with the explanation down below. And yes, we went with `strings` instead of `numbers`.


key | Meaning
---------- | -------
internal_server_error | It basically says it all, right - something went gravely wrong :)
conflict | Yes, you probably tried to send us the same request twice using the same `external_uid`. You know it has to be unique, right? :)
validation_failed |
service_unavailable |
not_found | Kinda represents the `404` of the error keys
account_locked | You probably did something nasty or just forgot your password :)
forbidden | Mmmm, you can't use this feature. Why you might ask - your account doesn't fulfill the prerequisites.
internal_server_error | `500` of the error codes
