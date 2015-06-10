#Transfer Approvals - Trust Account

> Response body

```
{
  "transfer_approval_id": "39",
  "transfer_type": "sepa_credit_transfer",
  "message": "Your transfer has been received and is waiting for approval."
}
```


> GET https://api.fidor.de/transfer_approvals

```
{
  "data": [
    {
      "id": 38,
      "transfer_id": "666",
      "transfer_type": "internal_transfer",
      "state": "approved",
      "created_at": "2015-06-10T08:10:33Z",
      "updated_at": "2015-06-10T08:10:33Z"
    },
    {
      "id": 39,
      "transfer_id": "",
      "transfer_type": "sepa_credit_transfer",
      "state": "rejected",
      "created_at": "2015-06-10T10:25:32Z",
      "updated_at": "2015-06-10T10:29:42Z"
    },
    {
      "id": 40,
      "transfer_id": "",
      "transfer_type": "sepa_credit_transfer",
      "state": "pending",
      "created_at": "2015-06-10T10:25:32Z",
      "updated_at": "2015-06-10T10:29:42Z"
    }
  ],
  "collection": {
    "current_page": 1,
    "per_page": 10,
    "total_entries": 2,
    "total_pages": 1
  }
}
```

Parameter | Description | Format
--------- | ----------- | -----------
id   | Unique identifier of the transfer approval | String
transfer_id | Unique identifier of the created transfer | String
transfer_type | Type of the transfer you tried to execute | String
state | State of the transfer approval | String, e.g. "pending", "approved", "rejected"
created_at | Creation date-time, never changes. ISO 8601 Date-Time e.g. "2014-10-10T17:41:58+02:00" | Datetime 
updated_at | Last update date-time. ISO 8601 Date-Time e.g. "2015-02-04T04:08:54+01:00" | Datetime

In case you're using Fidor's trust account e.g. for your crowdfunding platfom. Legally every trust account belongs to the Fidor Bank - it means that you cannot get the money from the account without Fidor's approval.

When you try to initiate an `internal_transfer` or a `sepa_credit_transfer` we will create a so called `transfer_approval` for you and block the transferred amount. After a `POST` to `internal_transfers` or a `sepa_credit_transfers` you will get the body of the `transfer_approval` created for you and the http status code `204`.

Once this `transfer_approval` has been approved, we will create and execute the transfer for you. If the `transfer_approval` was rejected the money will be credited back to the account.

You can get the information about the approvals by calling the `transfer_approvals` endpoint. You will see the `state` of the approval and the `transfer_id` in case your transfer has been approved.

HTTP REQUESTS

`GET https://api.fidor.de/transfer_approvals` <sub>index</sub>

`GET https://api.fidor.de/transfer_approvals/{id}` <sub>self</sub>