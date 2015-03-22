# Usage Examples

Following example is based on registration form documentation from Symfony2.

```
use Symfony\Component\HttpFoundation\Request;
// ...

public function createAction(Request $request)
{
    $form = $this->createForm(new RegistrationType(), new Registration());
    $form->handleRequest($request);

    if ($form->isValid()) {
        $registration = $form->getData();

        $transaction = $this->get('isolate')->getContext()->openTransaction();

        $transaction->persist($registration->getUser());

        $this->get('isolate')->closeTransaction();

        return $this->redirectToRoute(...);
    }

    return $this->render(
        'AcmeAccountBundle:Account:register.html.twig',
        array('form' => $form->createView())
    );
}
```

Of course before you can use Isolate transactions to store new entities in storage you need to
create entity definition first. More about how to create entity definitions can be found in
[Unit of Work documentation](../unit-of-work/entity-definition.md).
Also be sure that you know how to register entity definitions in unit of work which is described
in previous part of this documentation, [installation and configuration](installation-and-configuration.md).
