# Inconsistency use for 'Guard' function (using `require` vs `revert`)

Most of the time the project use `revert` for protecting the flow of logic, some part using `require`. Even though those two are similar usage, for handling the same type of situations, but `revert` is being used for with more complex logic. But if we look at contract codes, it fall into the same category, usage of both `require` and `revert`
