# Prisma Query

## 1. Pagination / Batch Query (next_discord) - [x]
```
await db.message.findMany({
	take: MESSAGES_BATCH,
	skip: 1,
	cursor: {
	  id: cursor,
	},
	where: {
	  channelId,
	},
	include: {
	  member: {
		include: {
		  profile: true,
		}
	  }
	},
	orderBy: {
	  createdAt: "desc",
	}
})
```

## 2. Select and Update with one query (Next Form Builder)
```
await prisma.form.update({
	data:{
	  submissions:{
		increment:1
	  },
	  FormSubmissions:{
		create:{
		  content,
		}
	  }
	},
	where:{
	  shareURL:formUrl,
	  published:true
	}
})
```

## 3. Basic: Create Many in one query (Next LMS)
```
await database.category.createMany({
	data: [
	  { name: "Computer Science" },
	  { name: "Music" },
	  { name: "Fitness" },
	  { name: "Photography" },
	  { name: "Accounting" },
	  { name: "Engineering" },
	  { name: "Filming" },
	]
})
```

## 4. Basic: Delete
```
await db.course.delete({
  where: {
	id: params.courseId,
  },
});
```

##  5. Basic: Filter with contain(like) keyword(NEXT LMS)
```
await db.course.findMany({
  where: {
	isPublished: true,
	title: {
	  contains: title,
	},
	categoryId,
  },
  include: {
	category: true,
	chapters: {
	  where: {
		isPublished: true,
	  },
	  select: {
		id: true,
	  }
	}
  },
  orderBy: {
	createdAt: "desc",
  }
});
```

## 6. Basic: Update
```
await db.chapter.update({
  where: {
	id: params.chapterId,
	courseId: params.courseId,
  },
  data: {
	isPublished: true,
  }
});
```

## 7. Advance: Upsert (combination of update and insert,Next LMS)
```
await prisma.model.upsert({
  where: { id: 1 }, // Check if a record with ID 1 exists
  update: { fieldToUpdate: 'new value' }, // If it exists, update this field
  create: { field1: 'value1', field2: 'value2' }, // If it doesn't exist, create a new record with these field values
});
```

## 8. Example: Find many with Not operation (Next Messeger Clone)
```
await prisma.user.findMany({
  orderBy: {
	createdAt: 'desc'
  },
  where: {
	NOT: {
	  email: session.user.email
	}
  }
});
```

## 9. Basic: FindUnique
```
await prisma.user.findUnique({
  where: {
	email: session.user.email as string,
  },
});
```

## 10. Example: Two level deep include
```
await prisma.conversation.findMany({
  orderBy: {
	lastMessageAt: 'desc',
  },
  where: {
	userIds: {
	  has: currentUser.id
	}
  },
  include: {
	users: true,
	messages: {
	  include: {
		sender: true,
		seen: true,
	  }
	},
  }
});
```

## 11. Basic: Connect a single record
```
await prisma.message.create({
  include: {
	seen: true,
	sender: true
  },
  data: {
	body: message,
	image: image,
	conversation: {
	  connect: { id: conversationId }
	},
	sender: {
	  connect: { id: currentUser.id }
	},
	seen: {
	  connect: {
		id: currentUser.id
	  }
	},
  }
});
```

## 12. Example: Connect to many records
```
await prisma.conversation.create({
	data: {
	  name,
	  isGroup,
	  users: {
		connect: [
		  ...members.map((member: { value: string }) => ({  
			id: member.value 
		  })),
		  {
			id: currentUser.id
		  }
		]
	  }
	},
	include: {
	  users: true,
	}
});
```

## 13. Basic: OR operator
```
await prisma.conversation.findMany({
  where: {
	OR: [
	  {
		userIds: {
		  equals: [currentUser.id, userId]
		}
	  },
	  {
		userIds: {
		  equals: [userId, currentUser.id]
		}
	  }
	]
  }
})
```
