# Bizmates Technical Exam, SQL EXAM

My answer for question number 1
```
SELECT 
Concat('T',LPAD(teacher.id,11,0)) as ID,
teacher.nickname as Nickname, 

CASE teacher.status
when '1' then 'Open'
when '2' then 'Backup'
when '3' then 'Reserved'
END as Status
,
CASE role.role 
when '1' then 'Trainer'
when '2' then 'Accessor'
when '3' then 'Staff'
END as Roles
FROM trn_teacher as teacher INNER JOIN trn_teacher_role as role on teacher.id = role.teacher_id
```

My answer for question number 2
```
SELECT
teacher.id as ID,
teacher.nickname as Nickname,
sum(CASE when teacher.status = 1 then 1 else 0 end) as Open,
sum(CASE when teacher.status = 3 then 1 else 0 end) as Reserved,
sum(CASE when evaluation.result = 1 then 1 else 0 end) as Taught,
sum(CASE when evaluation.result = 1 then 1 else 0 end) as "No Show"
FROM 
trn_teacher as teacher INNER JOIN trn_teacher_role as roles 
on teacher.id = roles.teacher_id 
INNER JOIN  trn_evaluation as evaluation
on teacher.id = evaluation.teacher_id
where
(teacher.status=1 or teacher.status=2)
```
