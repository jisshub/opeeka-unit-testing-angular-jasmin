**filtergrid.component.ts**

```ts
onChange($event: any) {
    if (this.headerFilterType !== 'number') {
      this.Msg = this.SetData(this.headerFilterType, this.headeritem, $event.target.value, 'Contains');
    } else {
      this.Msg = this.SetData(this.headerFilterType, this.headeritem, $event.target.value, 'Equals');
    }

    this.FilterCallBack.emit(this.Msg);


  }
  sendDate() {
    if (this.fromDate === null || this.fromDate === undefined) {
      if (isDateValid(this.toDate)) {
          const dateformt = moment(this.toDate).format('DD/MM/YYYY');
          this.Msg = this.SetData(this.headerFilterType, this.headeritem, dateformt, 'LessThanOrEqual');
      } else {
        this.Msg = this.SetData(this.headerFilterType, this.headeritem, '', 'LessThanOrEqual');
      }
    } else if (this.toDate === null || this.toDate === undefined) {
      if (isDateValid(this.fromDate)) {
          const dateformt = moment(this.fromDate).format('DD/MM/YYYY');
          this.Msg = this.SetData(this.headerFilterType, this.headeritem, dateformt, 'GreaterThanOrEqual');
      } else {
        this.Msg = this.SetData(this.headerFilterType, this.headeritem, '', 'GreaterThanOrEqual');
      }
    } else {
      if (isDateValid(this.fromDate) && isDateValid(this.toDate)) {
      const dateformt = moment(this.fromDate).format('DD/MM/YYYY') + '-' + moment(this.toDate).format('DD/MM/YYYY');
      this.Msg = this.SetData(this.headerFilterType, this.headeritem, dateformt, 'Between');
      } else {
        this.Msg = this.SetData(this.headerFilterType, this.headeritem, '', 'Between');
      }
    }
    this.FilterCallBack.emit(this.Msg);
  }
```

**filtergrid.spec.ts**

```ts
it('should raise FilterCallBack when onChange', () => {
  const onChangeSpy = spyOn(component.FilterCallBack, 'emit');
  component.onChange(EventEmitter);
  expect(onChangeSpy).toHaveBeenCalled();
});

it('should raise FilterCallback on sendDate', () => {
  const sendDateSpy = spyOn(component.FilterCallBack, 'emit');
  component.sendDate();
  expect(sendDateSpy).toHaveBeenCalled();
});
```

**Notes**:

The describe('UserSpeakComponent', () => ...) call is setting up a Test Suite for our User Speak Component. It will contain all the tests we wish to perform for our Component.

The beforeEach() calls specify code that should be executed before every test runs.

Each it() call creates a new test for the test runner to perform.

Now, we know our Component class has a constructor and two methods, sayHello and sayGoodbye.
As the constructor is empty, we do not need to test this

We can consider each of these methods to be units that need to be tested. Therefore we will write two unit tests for them.

It should be kept in mind that when we do write our unit tests, we want them to be isolated. Essentially this means that it should be completely self contained. If we look closely at our methods, you can see they are calling the emit method on the speak EventEmitter in our Component.

Our unit tests are not interested in whether the emit functionality is working correctly, rather, we just want to make sure that our methods call the emit method appropriately.

spyOn function which allows us to mock out the actual implementation of the emit call, and create a Jasmine Spy which we can then use to check if the emit call was made and what arguments were passed to it,

```ts
it('should set newDate to FromDate', () => {
  // Act
  component.onFromDateChange(new Date());

  // Assert
  expect(component.fromDate).toBe(new Date());
});
```

The first section is **Arrange**: Here you perform any set up required for your test.

The second section is **Act**: Here you will get your code to perform the action that you are looking to test.

The third and final sction is **Assert**: Here you will make verify that the unit performed as expected.

```ts
it('should raise FilterCallback on selectionChanged', () => {
  // Arrange
  const selectionChangedSpy = spyOn(component.FilterCallBack, 'emit');
  // Act
  component.selectionChanged(EventEmitter);
  // Assert
  expect(selectionChangedSpy).toHaveBeenCalled();
});
```

```ts
it('should return true', () => {
  // act
  const result = component.numberOnly(Event);
  // assert
  expect(result).toBeTruthy();
});

it('should return false', () => {
  const result = component.numberOnly(Event);
  expect(result).toBeFalsy();
});
```

**add-reportingunit.component.spec.ts**

```ts
it('should create a FormGroup comprised of FormControls', () => {
  component.initAddReportingUnit();
  expect(component.addReportingUnitForm instanceof FormGroup).toBe(true);
});
```

A FormGroup is a container for FormControls. When any of the FormControls in a FormGroup have an invalid state, the whole FormGroup is also invalid.

This test will simply call the _initAddReportingUnit_ method of our component class . Once that method completes, we just test that the _addReportingUnitForm_ attribute of the class is an instance of a FormGroup.

```ts
it('should call initAddReportingUnit and check properties are defined', () => {
  //   ARRANGE
  spyOn(component, 'initAddReportingUnit');

  // ACT
  component.ngOnInit();

  //   Assert
  expect(component.initAddReportingUnit).toHaveBeenCalled();
  expect(component.startDateLimit).toBeDefined;
  expect(component.minDate).toBeDefined;
  expect(component.maxDate).toBeDefined;
});
```

Use .toBeDefined to check that a variable is not undefined

Use .toHaveBeenCalled to ensure that a mock function got called.

You need to set up the spy before the method is called. Jasmine spys wrap function to determine when they are called and what they get called with. The method must be wrapped before the spied on method is called in order to capture the information

https://stackoverflow.com/a/42302967

```ts
it('should raise cancelSave on cancel', () => {
  const cancelSpy = spyOn(component.cancelSave, 'emit');
  component.cancel();
  expect(cancelSpy).toHaveBeenCalled();
});
```

```ts
it('should raise cancelSave when cancel', () => {
  const cancelSpy = spyOn(component.cancelSave, 'emit');
  const resetSpy = spyOn(component.addReportingUnitForm, 'reset');
  component.cancel();
  expect(cancelSpy).toHaveBeenCalled();
  expect(resetSpy).toHaveBeenCalled();
});

it('should call validateFullDate on ValidateDateFormat', () => {
  const spyOne = spyOn(component, 'validateFullDate');
  component.ValidateDateFormat(Event, Input, Type);
  expect(spyOne).toHaveBeenCalled();
  expect(spyOne).toHaveBeenCalledWith(Input);
});

it('should return false on ValidateDateFormat', () => {
  const result = component.ValidateDateFormat(Event, Input, Type);
  expect(result).toBeFalsy();
});

it('should set dateValid as false', () => {
  expect(component.dateValid).toBeFalsy();
});
```

**add-partner-agency-componenet.spec.ts**

```ts
it('should call initAddPartnerAgency on ngOnInit', () => {
  spyOn(component, 'initAddPartnerAgency');
  component.ngOnInit();
  expect(component.initAddPartnerAgency).toHaveBeenCalled();
  expect(component.isSharing).toBeTrue();
  expect(component.startDate).toBe(moment(new Date()));
});

it('should create a FormGroup comprised of FormControls', () => {
  component.initAddPartnerAgency();
  expect(component.addPartnerAgencyForm instanceof FormGroup).toBe(true);
});

it('should raise onCancelSave when cancel', () => {
  const cancelSpy = spyOn(component.onCancelSave, 'emit');
  const resetSpy = spyOn(component.addPartnerAgencyForm, 'reset');
  component.cancel();
  expect(cancelSpy).toHaveBeenCalled();
  expect(resetSpy).toHaveBeenCalled();
});

it('should set value on setToggle', () => {
  let value: any;
  spyOn(component, 'setToggle');
  component.setToggle(value);
  expect(component.setToggle).toHaveBeenCalled();
});
```

## Angular - Jest unit testing of method with parameter

https://stackoverflow.com/a/58818699

```ts
it('should set value on setToggle', () => {
  let value: any;
  spyOn(component, 'setToggle');
  component.setToggle(value);
  expect(component.setToggle).toHaveBeenCalled();
});
```

**add-collaboration.component**

```ts
it('addCollaborationunitForm invalid when empty', () => {
  expect(component.addCollaborationunitForm.valid).toBeFalsy();
});

it('should create a FormGroup comprised of FormControls', () => {
  component.initAddCollaborationUnit();
  expect(component.addCollaborationunitForm instanceof FormGroup).toBe(true);
});

it('Should make the collaorationID control valid and required', () => {
  let collaborationIdControl = component.collaborationID;
  expect(collaborationIdControl.valid).toBeFalsy();

  let errors = {};
  errors = collaborationIdControl.errors || {};
  expect(errors['required']).toBeTruthy();
});
```

here v check collabrpationid control is valid / not using valid property. so herev expect the control to be invalid at start.
can also test required attribute fails/not initially.

if collaborationControl has errors using errors property. then v expect errors object to contain a key 'required'. if required key is there then valdiator fails.

```ts
it('should set startDate and endDate to null', () => {
  expect(component.startDate).toBeNull();
  expect(component.endDate).toBeNull();
});
```

## How to test a function which has a setTimeout with jasmine?

https://stackoverflow.com/a/32499344

```ts
it('should push collaborationsData to collaborationsArray', () => {
  spyOn(component, 'disablecollaborations');
  expect(component.collaborationsArray).toBe([]);
  setTimeout(() => {
    expect(component.collaborationsArray).not.toBe([]);
  }, 0);
});
```

## services and components testing

**collaboration-add.component.ts**

```ts
constructor(
    public modalService: BsModalService,
  ){};

 openModal(template: TemplateRef<any>) {
    this.modalRef = this.modalService.show(template, {
      class: 'desclaimer-popup',
    });
  }
```

We have injected the _BsModalService_ Injectable into our Component.

**collaboration-add.component.spec.ts**

```ts
beforeEach(async(() => {
  TestBed.configureTestingModule({
    imports: [ReactiveFormsModule, FormsModule],
    declarations: [CollaborationAddComponent],
    providers: [
      {
        provide: BsModalService,
        useValue: {
          show: () => ({ class: 'desclaimer-popup' }),
        },
      },
    ],
    schemas: [NO_ERRORS_SCHEMA],
  }).compileComponents();
}));

it('modalRef should be defined', () => {
  const openModalSpy = spyOn(component.modalService, 'show');
  spyOn(component, 'openModal');
  expect(openModalSpy).toHaveBeenCalled();
});
```

We have to edit the TestBed set up to allow us to create the component correctly. Bear in mind, we are writing unit tests and therefore only want to run these tests in isolation and do not care if the _BsModalService_ methods are working correctly.

We are going to employ a technique in Unit Testing known as Mocking, to create a Mock Object, that will be injected to the component instead of the Real _BsModalService_.

The part we are interested in now is our _providers_ array. Here we are telling the compiler to provide the value defined here as the _BsModalService_. We set up a new object and define the method we want to mock out, in this case _show()_ and we will tell it a specific object to return.

Here we simply create a spy to ensure that the _show()_ call is made in the **openModal** methoid

## Modify our openMOdal() to handle errors:

**collobaration-add.ts**

```ts
 openModal(template: TemplateRef<any>) {
    try {
      this.modalRef = this.modalService.show(template, {
        class: 'desclaimer-popup',
      });
    } catch (error) {
      this.modalRef = null;
    }
  }
```

test will be like,

**collobaration-add.spec.ts**

```ts
it('should handle error when openModal', () => {
  const openModalSpy = spyOn(component.modalService, 'show').and.throwError(
    'Error'
  );
  spyOn(component, 'openModal');
  expect(openModalSpy).toHaveBeenCalled();
  expect(openModalSpy).toThrowError();
  expect(component.modalRef).toBe(null);
});
```

https://stackoverflow.com/a/59160364

**reviewer.componeny.ts**

### test 1

```ts
openRejectModal() {
    this.IsRejectOpen = true;
    this.confirmDialog.openModal();

  }
  openApproveModal() {
    this.IsApproveOpen = true;
    this.confirmDialog.openModal();
  }
```

**reviewer.componeny.spec.ts**

```ts
it('should raise confirmDialog on openModal', () => {
  const openRejectSpy = spyOn(component.confirmDialog, 'openModal');
  const openApprovalSpy = spyOn(component.confirmDialog, 'openModal');
  component.openRejectModal();
  expect(openRejectSpy).toHaveBeenCalled();
  expect(openApprovalSpy).toHaveBeenCalled();
  expect(component.IsRejectOpen).toBeTruthy();
  expect(component.IsApproveOpen).toBeTruthy();
});
```

### test 2

**reviewer.component.ts**

```ts
 ngOnInit(): void {
    this.assessmentByAssessmentId();
    this.initReview();
    this.getPersonSupports();
    this.getUserInfo();
  }
```

**reviewer.component.spec.ts**

```ts
it('should have been called at start', () => {
  component.ngOnInit();
  expect(component.assessmentByAssessmentId).toHaveBeenCalled();
  expect(component.initReview).toHaveBeenCalled();
  expect(component.getPersonSupports).toHaveBeenCalled();
  expect(component.getUserInfo).toHaveBeenCalled();
});
```

### test 3

**reviewer.component.ts**

```ts
initReview() {
    this.reviewNote = this.formBuilder.group({
      noteReviewText: new FormControl('', []),
    });
  }
```

**reviewer.component.spec.ts**

```ts
it('should create a FormGroup comprised of FormControl', () => {
  component.initReview();
  expect(component.reviewNote instanceof FormGroup).toBe(true);
});
```

### test 4

**reviewer.component.ts**

```ts
  openRejectModal() {
    this.IsRejectOpen = true;
    this.confirmDialog.openModal();

  }
  openApproveModal() {
    this.IsApproveOpen = true;
    this.confirmDialog.openModal();
  }
```

**reviewer.component.spec.ts**

```ts
it('should raise confirmDialog on openModal', () => {
  const openRejectSpy = spyOn(component.confirmDialog, 'openModal');
  const openApprovalSpy = spyOn(component.confirmDialog, 'openModal');
  component.openRejectModal();
  expect(openRejectSpy).toHaveBeenCalled();
  expect(openApprovalSpy).toHaveBeenCalled();
  expect(component.IsRejectOpen).toBeTruthy();
  expect(component.IsApproveOpen).toBeTruthy();
});
```

## Unit test for Switch Case

### test 5

**reviewer.component.ts**

```ts
getModalEvent(value) {
    if (value) {
      this.buttonDisable = true;
      // tslint:disable-next-line: radix
      const userid = parseInt(this.localStorageService.getDataFromLocalStorage(localStorageVariables.UserId));
      const assessmentReviewStatusData = {
        // tslint:disable-next-line: radix
        assessmentID: parseInt(this.assessmentId),
        assessmentStatus: this.reviewstatus,
        reviewNote: this.note.value,
        reviewUserID: userid,
      };
      switch (this.conformValues.event) {
        case 'Approved':
          this.getReviewStatus(assessmentReviewStatusData, this.ApproveSuccessMessage);
          break;
        case 'Returned':
          this.getReviewStatus(assessmentReviewStatusData, this.ReturnSuccessMessage);
          break;
        case 'Cancel':
          this.router.navigateByUrl('/notifications-all');
          break;
      }
    }
  }

```

**reviewer.component.spec.ts**

```ts
it('test Approved', () => {
  const getReviewStatusSpy = spyOn(component, 'getReviewStatus');
  expect(component.getModalEvent('Approved')).toEqual(getReviewStatusSpy);
});

it('test Returned', () => {
  const getReviewStatusSpy = spyOn(component, 'getReviewStatus');
  expect(component.getModalEvent('Returned')).toEqual(getReviewStatusSpy);
});

it('test Canceled', () => {
  const routerSpy = spyOn(component.router, 'navigateByUrl');
  expect(component.getModalEvent('Cancel')).toEqual(routerSpy);
});
```

---
