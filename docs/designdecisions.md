# Design Decisions

Used Eclipse Temurin for  java 17 -- open source that's why
for DataBase used My sql - not postgre yet to learn about that 
Created .gitignore file whith secerts like ignoring logs, database passwords etc because git history  is permanent!!
## DD-002: Separate SUBMITTED and IN_APPROVAL

SUBMITTED means the claim has successfully entered the
approval workflow but no approver has acted yet.

IN_APPROVAL means at least one approval action has occurred
and the workflow is still active.

Reason:
This distinguishes untouched submitted claims from claims
already progressing through the approval chain.
