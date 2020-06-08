# Permissions

User permissions in SmartRubric Assessment Core are worked out based on the groups to which the user belongs.

## Permissions and Default Values for GROUPS
| Title  | Permission | Default (Admin) | Default (Member) |
| ------------- | ------------- |------------- |------------- |
| individuals:admin | Access full personal information about Administrators of the group | None | None |
| individuals:members  | Access full personal information about Members of the group | Read/Write | No |
| memberlist  | Access the list of members of the group (names only) | Read/Write | Read |
| documents  | Access documents (such as assessments) created by others, connected to members of the group | Read/Write | None |
| mydocs  | Access documents (such as assessments) created by others, pertaining to one\s self | Read | Read |
| groupmetadata  | Access detailed metadata about the group | Read/Write | Read |
| subgrouplist  | Access the list of subgroups in the group | Read/Write | Read |

## Permission Inheritance

Permissions can cascade from group to subgroup, and you can choose to allow membership in one group to grant permissions in sibling groups.

> You can configure Read/Write permissions separately for both sibling and child inheritance. For example, if you enable Read inheritance but not Write inheritance, your the user will inherit read permissions only, but not write permissions.

| Title  | Permission | Default (Admin) | Default (Member) |
| ------------- | ------------- |------------- |------------- |
| Siblings | Access sibling groups with the same permissions. (For example, the teacher of 10EN1 can access 10EN2 as if they were a teacher of 10EN2) | Same Read access, no Write access | No |
| Children | Access subgroups of the group as if they were an administrator/member of that group. (For example, the Head of Year 10 has the same access to a Tutor Group in Year 10 as the Form Tutor.) | Read/Write | No |

  ### Use Case - Head of Year

  A **Head of Year** is an Administrator Member of a Year Group.

  Inside the Year Group there are 10 Tutor Groups, each with their own Administrator Members (Tutors) and Members (Students)  She needs to be able to read and write assessments and personal student information pertaining to the students in each Tutor Group.

  As a member of a senior leadership team, the Head of Year should be able to view but not necessarily update data pertaining to Tutors and Students in other Year Groups.

  *Therefore, when configuring the Year Group type, enable* **READ/WRITE** *access to child groups for administrators. Enable* **READ ONLY** *access to sibling groups.*

### Permission Cascades

**Child Inheritance works slightly differently to sibling inheritance.**

Because siblings are members of the same group type, permissions for administrators and members are identical. If one English teacher (administrator) accesses another English teacher's class, they will be accessing it as if they were the teacher of that class (although if you have read only inheritance, they will only inherit the read permissions)

Children of a group are a different group type to their parent group. (Year Group -> Tutor Group). So, using the example above, the Head of Year accesses a Tutor Group inside the Year Group in which she is a member with the same permissions as a Form Tutor (the administrator of a Tutor Group)--NOT with the permissions she has as a Head of Year.

This distinction is important because it means you can STOP a permission cascade:

| Title  | Administrator Name | Permission Flow Continues? | Notes |
| ------------- | ------------- |------------- |------------- |
| Local Authority | *Administrator* | NO | Local authority administrators can't see specific data from schools (aggregate only) |
| -> School | *School Leader* | READ ONLY | **Local authority** administrators have no access. **School Leaders** have Read Only access to all groups inside the school.  |
| --> Key Stage | *Head of Key Stage* | READ ONLY | **Local authority** administrators have no access. **School Leaders** have Read Only access to all Key Stages and Year Groups. **Heads of Key Stage** have READ/WRITE access to all Year Groups that belong to their Key Stage  |
| ---> Year Group | *Head of Year* | READ ONLY | **Local authority** administrators have no access. **School Leaders** have Read Only access to all Year Groups and Tutor Groups. **Heads of Key Stage** have READ/WRITE access to all Tutor Groups that belong to their Key Stage. **Heads of Year** have READONLY access to all Tutor Groups that belong to their year. |
| ----> Tutor Group | *Form Tutor* | NO | **Local authority** administrators have no access. **School Leaders** have Read Only access to all Tutor Groups and No access to student-led groups. **Heads of Key Stage** have READ/WRITE access to all Tutor Groups that belong to their Key Stage and No access to student-led groups. **Heads of Year** have READONLY access to all Tutor Groups that belong to their year and No access to student-led groups. **Form Tutors** have no access to student-led groups. |
| -----> Student Led Group | *Student* | NO | Only members of the group have access. Aggregate data only is available up the chain. The name of the group will be visible to anyone with Read access to the parent Tutor Group's ::subgrouplist:: permission|
