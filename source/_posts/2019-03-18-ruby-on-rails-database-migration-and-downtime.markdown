---
layout: post
title: "Ruby on Rails: Database Migration and Downtime"
date: 2019-03-18 10:20
comments: true
categories: "Ruby on Rails"
---

Recently, we had a production outage for a few minutes due a database
migration on one of our Ruby on Rails apps. The deployment went fine
through a few
stages, but the problem only showed up at the last and the largest
stage. This is exactly what happened during the deploy process.

1. New code was deployed. Restart was pending, so the server was still
running old code.
2. Migrations ran.
3. One migration removed a column that was used in the old code, but no
longer used in the new code.
4. The next migration was a data migration that inserted one row / user
to a table. This was a very slow migration, taking 5+ minutes.
5. The old code failed because it tried to use a column in the database
that was no longer there. To make things worse, the column was
referenced at all page loads within the app.
6. The long running migration didn't finish because it ran into a
timeout.
7. The servers weren't restarted because the migration had failed. So,
the new code wasn't served at all.
8. There was no automatic database rollback to restore the system into a good
state with the old code.

The team was able to resolve the issues within the next 5 minutes, but
it was the worst system outage we've seen in years. For anyone dealing
with a large Ruby on Rails app, you can use the following
safeguards to avoid such problems:

1. __Do not remove a column from the database while the current code is
still using it. Do it at a later release.__
2. When a deployment fails at the migration step, ensure you have a
rollback policy so that the system can be automatically restored to a
known good state.
3. Consider data migrations to be a performance problem and always test
the migrations with relaistic load before production release.
4. If possible, run your data migrations seprately from schema
migrations so that you don't incur deployment delays for optional new
data.


