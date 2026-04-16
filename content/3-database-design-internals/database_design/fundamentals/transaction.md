



> [!question] Does DB implicitly wraps a single query if the users doesn't uses transaction ?
> Yes, most modern relational databases automatically wrap individual queries in an implicit transaction if you do not define one yourself.
> This ensures ACID even on single query.







