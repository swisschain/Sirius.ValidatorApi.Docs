# Approval requests

Approval requests is what this API all about. Whenever a custody needs an approval from validators it initiates an approval request.
Each approval request object represents an overall process of approving of something. This object is a general one and it fits all the different
approval requests like root key initialization, transfer transaction, smart contract invocation and so on. This is achieved by having a `string context`
field that contains a approva process type-specific data serialized as a JSON object. Each validator sees its onw representation of an
approval request where he sees its own approval status and approval statuses of all other validators separately. Once an approval request
is approved or rejected it can't become a pending again.