---
layout: post
title: Upgrading NHibernate from 1.2 to 2.1
categories: []
tags:
- Uncategorized
status: draft
type: post
published: false
meta: {}
---
- Replace the assemblies
- Update the references
- Replace all EqExpression() with Restrictions.Eq()

New NHibernate.Expression.EqExpression("Name", value)
NHibernate.Criterion.Restrictions.Eq("Name", value)

- Update the config file to define which proxy factory is being used (need to place it with the other properties)

&lt;property name='proxyfactory.factory_class'&gt;NHibernate.ByteCode.Castle.ProxyFactoryFactory, NHibernate.ByteCode.Castle&lt;/property&gt;
<a href="http://davybrion.com/blog/2009/03/upgrading-to-nhibernate-21/">http://davybrion.com/blog/2009/03/upgrading-to-nhibernate-21/</a>

- Make all domain object methods virtual (use find/replace to avoid RSI!)

- Update the session object to use the web session storage or per thread storage, depending on the target app.
