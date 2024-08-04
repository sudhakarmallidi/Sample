class ShowOverlayTests: XCTestCase {

    func testShowOverlayWithContainerView() {
        // Arrange
        let containerView = UIView(frame: CGRect(x: 0, y: 0, width: 100, height: 100))
        let overlayColor = UIColor.red
        let expectation = self.expectation(description: "Completion handler called")

        // Act
        showOverlay(on: containerView, overlayColor: overlayColor) {
            // Assert
            XCTAssertEqual(containerView.subviews.count, 1)
            let overlayView = containerView.subviews.first
            XCTAssertEqual(overlayView?.backgroundColor, overlayColor)
            XCTAssertEqual(overlayView?.frame, containerView.bounds)
            expectation.fulfill()
        }

        waitForExpectations(timeout: 1, handler: nil)
    }

    func testShowOverlayWithoutContainerView() {
        // Arrange
        let overlayColor = UIColor.blue
        let expectation = self.expectation(description: "Completion handler called")

        // Mock UIWindow to use as the main window
        let window = UIWindow(frame: UIScreen.main.bounds)
        UIApplication.shared.delegate?.window = window

        // Act
        showOverlay(overlayColor: overlayColor) {
            // Assert
            XCTAssertEqual(window.subviews.count, 1)
            let overlayView = window.subviews.first
            XCTAssertEqual(overlayView?.backgroundColor, overlayColor)
            XCTAssertEqual(overlayView?.frame, UIScreen.main.bounds)
            expectation.fulfill()
        }

        waitForExpectations(timeout: 1, handler: nil)
    }

    func testShowOverlayCallsCompletion() {
        // Arrange
        let expectation = self.expectation(description: "Completion handler called")

        // Act
        showOverlay(overlayColor: .clear) {
            // Assert
            expectation.fulfill()
        }

        waitForExpectations(timeout: 1, handler: nil)
    }
}
